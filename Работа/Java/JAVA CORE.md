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
## Arrays
Шпаргалка по командам
```java
Array.binarySearch()
Array.equals()
Array.fill()
Array.sort()
Array.parallelSort()
Array.stream()

```
## Stream API


## Var
Локальные переменные, параметры можно объявлять как var, компилятор сам решит, что нужно подставить

## Control flow
- if-then
- if-then-else
- while/do-while
- for
- switch

## Switch
```java
//old
// switch statement
int selector = // 1-10
switch (selector) {
	case 1: do something;
	case 2: do something;
	case 3: do something;
	case 3: do something;
	case 4: do something;
	case 5: do something;
	case 6: do something;
	default: do something; 
}

//new
// switch became an expression
// use yield to exit from switch expression
public String convertToLabel(int quarter) {
    String quarterLabel =
        switch (quarter) {
            case 0  -> {
                IO.println("Q1 - Winter");
                yield "Q1 - Winter";
            }
            default -> "Unknown quarter";
        };
    }
    return quarterLabel;
}
```

> [!info] Important
> These primitive types cannot be used for the type of your selector variable: 
> - `boolean`, 
> - `long`, 
> - `float`, 
> - `double`.

> [!info] Important
> Switch statement (old) use a **drop-through style**. it means that if one case became true all others also will be executed if there is not a break key word

## Classes
```java
public/private Bicycle {
	
	/*
		Field declarations are composed of three components, in order:
			1. Zero or more modifiers, such as `public`, `private`, `final` .
			2. The field's type.
			3. The field's name.
	*/
	private int model;
	private String info;
	public float speed;
	
	// constractor
	public Bicycle (int speed, String model) {
	// etc
	}
	
	// Methods
	// Note: the first word should be always a verb
	public void changeSpeed(int newSpeed) {
	// etc
	}
	public void changeInfo(String newInfo) {
	// etc
	}
}
```

## Garbage collector (GC)

>[!info] Important
>An object is eligible for garbage collection when there are no more references to that object
>![[Screenshot 2026-08-22 at 16.44.21.png]]


## Access to class
![[Screenshot 2026-08-22 at 16.48.54.png|500]]


## Static and Non-static initialization
```java
public Bicycle implements BicycleInterface {
	// fields
	

	static {
	// this block is being executed once in runtime when the class created
	}	
	
	// this block is being copied to all constractor and executed first
	{
	
	}
	
	// constractors
	public Bicycle (float speed, String model) {
	// etc
	}
	
	public Bicycle (String model) {
	// etc
	}
}
```

## Nested class
```java
class Person {
	class Man {
	//etc
	}
	
	class Woman {
	//etc
	}
}
```

[About nested classes](https://dev.java/learn/classes-objects/nested-classes/)

## [[Stream API]]
