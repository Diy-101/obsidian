Alembic — это инструмент для управления миграцией баз данных. 

```bash
# install
pip install alembic

# Initialization
alembic init alembic # В корни проекта

# Появится следующая структура
alembic/
  versions/        # файлы миграций
  env.py           # логика подключения к БД
  script.py.mako
alembic.ini        # конфигурация

# Создания миграции
"""
Использую, когда:
	добавил / изменил / удалил модель
    поменял поле, индекс, constraint
"""
alembic revision --autogenerate -m "описание изменений"

# Применение миграции
alembic upgrade head      # применить все новые миграции
alembic upgrade +1        # применить одну миграцию
alembic upgrade <rev_id>  # перейти к конкретной версии

# Откат миграции
alembic downgrade -1        # откатить одну миграцию
alembic downgrade base      # откатить всё
alembic downgrade <rev_id>  # откатиться к версии

# Работа с текущей версией
alembic current     # текущая версия БД
alembic heads       # последние версии (heads)
alembic history     # история всех миграций

# Работа с указателем HEAD
alembic stamp head # сдвинуть указатель на последнее изменение (победить БД как актуальную)

```

Алембик всегда работает в синхронном режиме. При создании миграции нужно учитывать различие драйверов для баз дынных. Все ассинхронные драйверы нужно переводить в синхронные. 
Частый кейс:
- asyncpg — psycopg2

