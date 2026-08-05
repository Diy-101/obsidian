```java
// Конструкция try-with-resources позволяет закрывать ресурсы автоматически

try (Type resource1 = ...; Type resource1 = ...; Type resource1 = ...) {
	// работа с ресурсами
} catch (Exception e) {  
    // действия если поймали ошибку 
}
```