## Object
- Fields — state
- methods — behavior

Объекты в java — это пример инкапусуляции, одного из принципов ООП.
![[Screenshot 2026-08-17 at 16.23.27.png]]
Наследование от классов происходит с помощью ключевого слова `extends`
```java
class MotorCycle extends Cycle {
	// only new field and methods
}
```
![[Screenshot 2026-08-17 at 16.24.54.png]]Java предоставляет инструмент для создания контракта между объектом и его поведением — **интерфейсы**
```java
interface Cycle {
	// methods that should be implemented
	// in class
	void ride (int speed);
	
	void turnRight (float angle);
	
	void turnLeft (float angle);
	
	void stop ();
}
```
Чтобы класс реализовывал методы интерфейса используется ключевое слово `implements`
```java
class MotorCycle implements Cycle {
	int speed = 0;
	float angle = 0.0;
	
	// The compiler will now require those methods 
	public void ride (int speed) {
		// implementation
	};
	
	public void turnRight (float angle) {
		// implementation
	};
	
	public void turnLeft (float angle) {
		// implementation
	};
	
	public void stop () {
		// implementation
	};
}
```
В java присутствует 4 видов переменных:
- Instance (Non-static) variables — переменные самого инстанса из класса
- Class (static) variables — переменные класса
- Local variables — локальные переменные метода
- Parameters — переменные метода. По своей сути они являются посредниками, передающими данные между цепочкой вызовов методов.
## Primitive types (8)
- **byte** — 8 bit signed
- **short** — 16 bit signed
- **int** — 32 bit signed
- **long** — 64 bit signed
- **float** — 32 bit signed
- **double** 64 bit signed
- **boolean** — 1 bit (true — 1, false — 0)
- **char** — 16 bit unicode character (from `\u0000 (0)` to `\nffff (65,535)`) 
