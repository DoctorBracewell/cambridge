**Name:** Brace Godfrey
**CRSId:** dg681
### 23
> Explain the result of the following code. ![[Pasted image 20231118010207.png]]

The first output from the code is `1 1`.
This is because the first call to `add` uses the implementation that takes two integers and adds two numbers to them. The `x` and `y` parameters supplied are integers taken from the `xypair` array, but these primitive values are copied when they are passed into the function.
However, this implementation mutates the `x` and `y` parameters. Since the parameters are also passed by-value, this has no effect on the original `xypair` array that the values were taken from, so the println outputs the original form of the array, `1 1`.

The second output from the code is `2 2`. This is because the `xypair` array is passed in as a reference to the `add` function. This function actually mutates the item in the array, and can do this because it has a copy of the reference to the original array. When the array is printed out after the function the values have been changed.
### 24
> A student wishes to create a class for a 3D vector and chooses to derive from the `Vector2D` class (i.e. `public void Vector3D extends Vector2D`). The argument is that a 3D vector is a “2D vector with some stuff added”. Explain the conceptual misunderstanding here.

Inheritance is usually understood as an "is-a" relationship. A child class of a parent can be used in any location that the parent class can. This does not apply to 3D and 2D vector classes - although they may share similar state such as `x` and `y` variables or methods such as magnitude, a 3D vector cannot be used in the same places a 2D vector can - in multiplying with other vectors, for example.
### 25
> If you don’t specify an access modifier when you declare a member field of a class, what does Java assign it? Demonstrate your answer by providing minimal Java examples that will and will not compile, as appropriate.

Java will allow the member to be accessed in the class and in the package, but not in any sub-classes. 

```java
// Will compile
class MyClass {
	String myField = "Hello world.";

	public void test() {
		System.out.println(myField);
	}
}
```

```java
// Won't compile
class MyClass {
	String myField = "Hello world.";
}

class MySubClass extends MyClass {
	public void test() {
		System.out.println(myField);
	}
}
```
### 26
> Suggest UML class diagrams that could be used to represent the following. Think carefully about the directions of and multiplicities on your arrows.

> a) A shop is composed of a series of departments, each with its own manager. There is also a store manager and many shop assistants. Each item sold has a price and a tax rate.

> b) Vehicles are either motor-driven (cars, trucks, motorbikes, electric bikes) or human-powered (bikes, skateboards, scooters). All cars have 3 or 4 wheels and all bikes have two wheels. Every vehicle has an owner. Some vehicles must have road tax.


### 27
> Consider the Java class below:
> ![[Pasted image 20231118174038.png]]

