
## Access Modifiers
Protected fields are fields that can be accessed in the class that it is declared in and any subclasses/child classes that is inherited from.


Method overriding (@Override), this is when we override the method of the parent class in the child class. 


### Dependency Injection

Dependency injection simply details the class in question with its relevant dependencies. The class lists its dependencies which are then provided through parameters, thereby not concretely instantiating and creating the classes inside of the class in question in of itself. 


There are 3 forms of DI:
 - Constructor Injection
 - Setter Injection
 - Method Injection

###  Constructor injection in DI:

```java
public class TaxReport {
	private TaxCalculator calculator;

	public TaxReport(TaxCalculator calculator) {
		this.calculator = calculator
	}

	public void show() {
		var tax = calculator.calculateTax();
		System.out.println(tax);
	}
}
```

 In the main class, an object can now be provided as a concrete implementation and 'injected' right into the TaxReport. This allows the class to easily adapt to any changes and you don't need to change any of the implementation detail of TaxReport.
i.e. 

_Main.java_
```java
package com.helloworld

public class Main {

	public static void main(String[] args) {
		var calculator = new TaxCalculator2018(100_000)
		var report = new TaxReport(calculator);
	}
}
```

### Setter injection in DI:

A setter injection is when the class in question accepts an injection through its setter implementation. 

Thus, the class in question can accept a constructor injection _and_ a setter injection. If the class needs a different implementation throughout its lifecycle, this could be handy:

```java
public class Main {
	public static void Main(string[] args) {
		var calculator = new TaxCalculator2018(100_000);
		var report = new TaxReport(calculator);

		// now we introduce a different implementation

		report.setCalculator(new TaxCalculator2019(100_000));
		report.show();
	}
}
```


### Method Injection

In method injection, we take a different approach and directly inject dependencies into the method that we are concerned about. This ensures that we don't have to call a setter at runtime. This could create duplication or further complexity, so constructor injection is still preferred.

i.e. 

```java
public class Main {
	public static void Main(string[] args) {
		var calculator = new TaxCalculator2018(100_000);
		var report = new Report()

		// we directly inject the class into the method
		report.show(calculator);
	}
}
```


### Interface Segregation Principle

The interface segregation principle is very much like the Single Responsibility principle from the SOLID acronym. It's best to keep interfaces as lightweight and modular as possible to ensure that the amount of coupling points remain minimal.

To illustrate, lets take a UIWidget which is an interface, thereby a contract with multiple different methods attached to it.

*UIWidget.java*

```java
package com.presentation

public interface UIWidget {
	void drag();
	void resize();
	void render();
}
```

And we have a concrete implementation of this, called Dragger 


*Dragger.java*
```java
package com.presentation


public class Dragger {

	@Override
	public void drag(UIWidget widget) {
		widget.drag();
		// any further behaviour
	}
}
```

Now if any part of this interface changes, it will cause a recompilation of those classes and now this `drag` method has superfluous knowledge of the `resize` method. 

Instead, break out the `UIWidget` interface into smaller interfaces that are split by **capability**. 

_draggable.java_
```java
public interface Draggable {
	void drag()
}
```


and if the UIWidget needs knowledge of drag, it can be now inherited from the Draggable interface:

_UIWidget.java_

```java
package com.presentation

public interface UIWidget extends Draggable {
	void resize(int x, int y, int width, int height);
	void render();
}
```

And now `Dragger.java` or any other concerned/dependent classes upon the contract will have those in their visible methods.

Additionally, this brings about the topic of multiple inheritance with interfaces. As interfaces do not have concrete implementations of their associated methods, ambiguity of class and method selection are resolved. Therefore, multiple inheritance of interfaces are perfectly allowed. This further lends to the modularity of the principle.

This can be illustrated as so:

_UIWidget.java_
```java
public interface UIWidget extends Draggable, Resizable {
	void render();
	void close();
}
```

Therefore, to conclude, interface segregation principle is the essence of the single responsibility principle applied to interfaces.

