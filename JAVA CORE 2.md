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














