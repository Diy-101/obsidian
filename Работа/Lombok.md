Библиотека для сокращение кодовой базы с помощью аннотаций

## @Getter @Setter
```java
@Getter
@Setter
public class Author {    
	private int id;    
	private String name;    
	@Setter(AccessLevel.PROTECTED)    
	private String surname;
}
```

## @NoArgsConstructor @RequiredArgsConstructor 
## @AllArgsConstructor

![[Screenshot 2026-09-15 at 16.22.38.png]]

**RequiredArgsConstructor** — Добавляет конструктор для @NotNull и final полями