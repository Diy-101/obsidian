## Что такое `EXISTS`?

**В SQL:**

`EXISTS` — это оператор, который проверяет:  
**существует ли хотя бы одна строка**, подходящая под подзапрос.
```sql
SELECT EXISTS (
     SELECT 1     
     FROM users_promos     
     WHERE user_id = :user_id      
      AND promo_id = :promo_id 
);

Вернёт `true` или `false`.
```

**Главная идея:**
> Мне не важно _сколько_ строк, мне важно — **хотя бы одна есть или нет**.

Это часто эффективнее, чем `COUNT(*)`, потому что БД может остановиться на первой найденной строке.

**В SQLAlchemy:**
```python
from sqlalchemy import select, exists

stmt = select(
    exists().where(
        PromoUserORM.user_id == user_id,
        PromoUserORM.promo_id == promo_id,
        PromoUserORM.liked.is_(True),
    )
)

result: bool = await db.scalar(stmt)
```

## Виды JOINS в SQL и SQLAlchemy
### INNER JOIN
==INNER JOIN== — возвращает только те строки, **ключ** которых существует в двух таблицах
```python
query = (
	select(PromocodeORM, PromoUserORM)
	.join(PromoUserORM, PromocodeORM.id == PromoUserORM.promo_id)
)
```

### LEFT JOIN
==LEFT JOIN== — возвращает все строки из левой таблицы и совпадающие строки из правой
```python
query = (
	select(PromocodeORM, PromoUserORM)
	.outerjoin(PromoUserORM, PromocodeORM.id == PromoUserORM.promo_id)
)

# ИЛИ
query =(
	select(PromocodeORM, PromoUserORM)
	.join(PromoUserORM, PromocodeORM.id == PromoUserORM.promo_id, isouter=True)
)
```

Используется, когда нужно вернуть модель в **любом случае**

### FULL OUTER JOIN

==FULL OUTER JOIN== — Возвращает все строки из двух таблиц

```python
query = (
	select(PromocodeORM, PromoUserORM)
    .join(
        PromoUserORM,
        PromocodeORM.id == PromoUserORM.promo_id,
        full=True
    )
)

"""
SELECT *
FROM promos
FULL OUTER JOIN users_promos ON promos.id = users_promos.promo_id;
"""
```

### CROSS JOIN
==CROSS JOIN== — Возвращает декартово произведение таблиц. То есть **N x M** записей

```python
query = (
	select(PromocodeORM, UserORM)
	.select_from(
		UserORM.crossjoin(PromocodeORM)
	)
)
```

## Типы связей 
### 1 : N (один ко многим)
компания → промокоды.
```python
class CompanyORM(Base):
    __tablename__ = "companies"

    id: Mapped[str] = mapped_column(primary_key=True)
    # ...
    promos: Mapped[list["PromocodeORM"]] = relationship(back_populates="company")


class PromocodeORM(Base):
    __tablename__ = "promos"

    id: Mapped[str] = mapped_column(primary_key=True)
    company_id: Mapped[str] = mapped_column(ForeignKey("companies.id"))
    company: Mapped["CompanyORM"] = relationship(back_populates="promos")

```

### M : N (многие ко многим)

```python
# Простой вариант
user_promo_m2m = Table(
    "users_promos",
    Base.metadata,
    Column("user_id", ForeignKey("users.id"), primary_key=True),
    Column("promo_id", ForeignKey("promos.id"), primary_key=True),
)

class UserORM(Base):
    __tablename__ = "users"
    id: Mapped[str] = mapped_column(primary_key=True)
    promos: Mapped[list["PromocodeORM"]] = relationship(
        secondary=user_promo_m2m,
        back_populates="users",
    )

class PromocodeORM(Base):
    __tablename__ = "promos"
    id: Mapped[str] = mapped_column(primary_key=True)
    users: Mapped[list["UserORM"]] = relationship(
        secondary=user_promo_m2m,
        back_populates="promos",
    )

# Сложный вариант, когда нужны дополнительные поля

class UserORM(Base):
    __tablename__ = "users"
    id: Mapped[str] = mapped_column(primary_key=True)
    user_promos: Mapped[list["PromoUserORM"]] = relationship(back_populates="user")

class PromocodeORM(Base):
    __tablename__ = "promos"
    id: Mapped[str] = mapped_column(primary_key=True)
	user_promos: Mapped[list["PromoUserORM"]] = relationship(back_populates="promo")

class PromoUserORM(Base):
    __tablename__ = "users_promos"

    user_id: Mapped[str] = mapped_column(
        ForeignKey("users.id", ondelete="CASCADE"), primary_key=True
    )
    promo_id: Mapped[str] = mapped_column(
        ForeignKey("promos.id", ondelete="CASCADE"), primary_key=True
    )

    activated: Mapped[bool] = mapped_column(server_default="false")
    liked: Mapped[bool] = mapped_column(server_default="false", nullable=False)

    user: Mapped["UserORM"] = relationship(back_populates="user_promos")
    promo: Mapped["PromocodeORM"] = relationship(back_populates="user_promos")

```

## Полезные функции и паттерны

```python
# select_from — когда нужно явно указать источник
query = (
	select(PromocodeORM, UserORM)
	.select_from(
		UserORM.crossjoin(PromocodeORM)
	)
)

# func — прокси для sql функций

func.count()      # -> COUNT(*)
func.now()        # -> NOW()
func.lower(col)   # -> LOWER(col)

# func.count()
stmt = select(func.count()).select_from(PromoUserORM)
total = await db.scalar(stmt)

stmt = select(
    func.count().filter(PromoUserORM.liked.is_(True))
)
select(func.count()).select_from(User) # Подсчитать все записи

# func.now()
created_at: Mapped[datetime] = mapped_column(
    server_default=func.now()
)

stmt = select(PromocodeORM).where(
    PromocodeORM.active_until > func.now()
)

# func.coalesce()
stmt = select(
    func.coalesce(PromocodeORM.like_count, 0).label("like_count")
)

# Строковые функции
func.lower(UserORM.email)       # LOWER(email)
func.upper(UserORM.name)        # UPPER(name)
func.trim(UserORM.name)         # TRIM(name)
func.concat(UserORM.name, " ", UserORM.surname)

# Временные функции
func.date_trunc("day", PromocodeORM.active_from)
func.age(PromocodeORM.active_until)


# первые N строк
select(User).limit(N)

# N строк, начиная с K-й (с нуля)
select(User).limit(N).offset(K)
```