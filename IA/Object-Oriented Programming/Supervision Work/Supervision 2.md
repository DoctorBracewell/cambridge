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
> Another class `Y` attempts to access the field value in an object of type `X`. Describe what happens at compilation and/or runtime for the range of MODIFIER possibilities (i.e. public, protected, private and unspecified) under the following circumstances:
> (a) Y subclasses X and is in the same package; 
> (b) Y subclasses X and is in a different package; 
> (c) Y does not subclass X and is in the same package; 
> (d) Y does not subclass X and is in a different package.

a) Can be accessed by `public`, `protected`, `private` and unspecified.
b) Can be accessed by `public, protected` and unspecified.
c) Can be accessed by `public` and unspecified.
d) Can be accessed only by `public`.
### 28
> Explain what is meant by (dynamic) polymorphism in OOP and explain why it is useful, illustrating your answer with an example.

Polymorphism is when objects have the same methods with different implementations, so they fit under one parent type. Dynamic polymorphism is determined at runtime, and the child method can be run instead of the default parent method.
### 29
> A programming language designer proposes adding ‘selective inheritance’ whereby a programmer manually specifies which methods or fields are inherited by any subclasses. Comment on this idea.

I think this is a bit of a silly idea. It could lead to very complicated class hierarchies that are hard to maintain or understand. It also violates the substitution principle, because a subclass might not be able to replace an instance of the parent class.
### 30
> A Computer Science department keeps track of its CS students using some custom software. Each student is represented by a `Student` object that features a `pass()` method that returns true if and only if the student has all 20 ticks to pass the year. The department suddenly starts teaching NS students, who only need 10 ticks to pass. Using inheritance and polymorphism, show how the software can continue to keep all Student objects in one list in code without having to change any classes other than Student.

```java
abstract class Student {  
    protected int ticksRequired;  
    protected int currentTicks;  
  
    public boolean pass() {  
        return currentTicks >= ticksRequired;  
    }  
}  
  
class ComputerScienceStudent extends Student {  
    ComputerScienceStudent() {  
        ticksRequired = 20;  
    }  
}  
  
class NaturalScienceStudent extends Student {  
    NaturalScienceStudent() {  
        ticksRequired = 10;  
    }  
}

// Later...
ArrayList<Student> students = new ArrayList<Student>();
```
### 31
> Complete the Chess practical exercise on Chime.


```java
// Piece.java 
abstract public class Piece {  
  protected Position position;  
  protected final PieceColor pieceColor;  
  protected final Board board;  
  protected final char name;  
  protected char icon = '-';  
  protected int value = 0;  
  
  public Piece(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
    this.name = name;  
    this.position = piecePosition;  
    this.pieceColor = pieceColor;  
    this.board = board;  
  }  
  
  
  public char icon() {  
    return icon;  
  }  
  
  public int value() {  
    return value;  
  }  
  
  public Position position() {  
    return position;  
  }  
  
  public char name() {  
    return name;  
  }  
  
  public void moveTo(Position newPosition) {  
    position = newPosition;  
  }  
  
  public PieceColor colour() {  
    return pieceColor;  
  }  
  
  public String toString() {  
    return name() + " " + position.toString();  
  }  
  
  public Board board() {  
    return board;  
  }  
  
  abstract public List<Position> validNextPositions();  
}
```

```java
public class Bishop extends Piece {  
    public Bishop(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
        super(name, piecePosition, pieceColor, board);;  
        this.icon = pieceColor == PieceColor.BLACK ? '♝' : '♗';  
        this.value = 3;  
    }  
  
    public List<Position> validNextPositions() {  
        List<Position> nextPositions = new ArrayList<>();  
  
        position.getAllDiagonalMoves(8, board(), nextPositions);  
  
        return nextPositions;  
    }  
}
```

```java
public class King extends Piece {  
    public static int KING_VALUE = 10000;  
  
    public King(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
        super(name, piecePosition, pieceColor, board);  
        this.icon = pieceColor == PieceColor.BLACK ? '♚' : '♔';  
        this.value = KING_VALUE;  
    }  
  
    public List<Position> validNextPositions() {  
        List<Position> nextPositions = new ArrayList<>();  
  
        position.getAllDiagonalMoves(1, board(), nextPositions);  
        position.getAllStraightMoves(1, board(), nextPositions);  
  
        return nextPositions;  
    }  
}```

```java
public class Knight extends Piece {  
    public Knight(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
        super(name, piecePosition, pieceColor, board);  
        this.icon = pieceColor == PieceColor.BLACK ? '♞' : '♘';  
        this.value = 3;  
    }  
  
    public List<Position> validNextPositions() {  
        List<Position> nextPositions = new ArrayList<>();  
  
        computeKnightNextPositions(nextPositions);  
        position.getAllDiagonalMoves(8, board(), nextPositions);  
  
        return nextPositions;  
    }  
  
    private void computeKnightNextPositions(List<Position> nextPositions) {  
        // directions a knight can travel in.  
        final int[][] nextPosDeltas =  
                new int[][]{  
                        {1, 2}, {1, -2}, {-1, 2}, {-1, -2},  
                        {2, 1}, {-2, 1}, {2, -1}, {-2, -1}  
                };  
  
        // iterate through all possible positions, getting any valid next positions  
        for (int[] posDeltaPair : nextPosDeltas) {  
            position.addPosAtDelta(posDeltaPair[0], posDeltaPair[1], board(), nextPositions);  
        }  
    }  
}
```

```java
public class Knight extends Piece {  
    public Knight(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
        super(name, piecePosition, pieceColor, board);  
        this.icon = pieceColor == PieceColor.BLACK ? '♞' : '♘';  
        this.value = 3;  
    }  
  
    public List<Position> validNextPositions() {  
        List<Position> nextPositions = new ArrayList<>();  
  
        computeKnightNextPositions(nextPositions);  
        position.getAllDiagonalMoves(8, board(), nextPositions);  
  
        return nextPositions;  
    }  
  
    private void computeKnightNextPositions(List<Position> nextPositions) {  
        // directions a knight can travel in.  
        final int[][] nextPosDeltas =  
                new int[][]{  
                        {1, 2}, {1, -2}, {-1, 2}, {-1, -2},  
                        {2, 1}, {-2, 1}, {2, -1}, {-2, -1}  
                };  
  
        // iterate through all possible positions, getting any valid next positions  
        for (int[] posDeltaPair : nextPosDeltas) {  
            position.addPosAtDelta(posDeltaPair[0], posDeltaPair[1], board(), nextPositions);  
        }  
    }  
}
```

```java
public class Pawn extends Piece {  
    public Pawn(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
        super(name, piecePosition, pieceColor, board);  
        this.icon = pieceColor == PieceColor.BLACK ? '♟' : '♙';  
        this.value = 1;  
    }  
  
    public List<Position> validNextPositions() {  
        List<Position> nextPositions = new ArrayList<>();  
  
        computePawnNextPositions(nextPositions);  
        position.getAllDiagonalMoves(8, board(), nextPositions);  
  
        return nextPositions;  
    }  
  
    private void computePawnNextPositions(List<Position> nextPositions) {  
        // The En passant move is not included.  
        // The Promotion is not included.  
    /*    pawns can move up 1 if it is a non-occupied square.    pawns can move (and take) up 1 and left or right 1 if the square is occupied by an opponent    pawns can move up 2 if they are currently on their home row (pawns cannot go backwards)  
     */  
        int upDir = (pieceColor == WHITE ? 1 : -1);  
  
        // move up by one  
        addPawnPositionIfValid(upDir, 0, false, nextPositions);  
  
        // move up left and right  
        addPawnPositionIfValid(upDir, -1, true, nextPositions);  
        addPawnPositionIfValid(upDir, 1, true, nextPositions);  
  
        // move up two if on their home row  
        if (position().getRank() == (colour() == BLACK ? SEVEN : TWO)) {  
            addPawnPositionIfValid(upDir + upDir, 0, false, nextPositions);  
        }  
    }  
  
    private void addPawnPositionIfValid(  
            int deltaRank,  
            int deltaFile,  
            boolean allowIfOccupiedByOpponent,  
            List<Position> nextPositions) {  
  
        Position movePosition = position().getPosAtDelta(deltaRank, deltaFile);  
  
        if (movePosition != null) {  
            boolean movePosOccupied = board().positionOccupied(movePosition);  
            // the up left and right cases  
            if (allowIfOccupiedByOpponent  
                    && movePosOccupied  
                    && board().atPosition(movePosition).colour() != colour()) {  
                nextPositions.add(movePosition);  
            }  
            // the "up straight ahead" and "up straight ahead two" moves.  
            else if (!allowIfOccupiedByOpponent && !movePosOccupied) {  
                nextPositions.add(movePosition);  
            }  
        }  
    }  
}
```

```java
public class Queen extends Piece {  
    public Queen(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
        super(name, piecePosition, pieceColor, board);  
        this.icon = pieceColor == PieceColor.BLACK ? '♛' : '♕';  
        this.value = 10;  
    }  
  
    public List<Position> validNextPositions() {  
        List<Position> nextPositions = new ArrayList<>();  
  
        position.getAllDiagonalMoves(8, board(), nextPositions);  
        position.getAllStraightMoves(8, board(), nextPositions);  
  
        return nextPositions;  
    }  
}
```

```java
public class Rook extends Piece {  
    public Rook(char name, Position piecePosition, PieceColor pieceColor, Board board) {  
        super(name, piecePosition, pieceColor, board);  
        this.icon = pieceColor == PieceColor.BLACK ? '♜' : '♖';  
        this.value = 5;  
    }  
  
    public List<Position> validNextPositions() {  
        List<Position> nextPositions = new ArrayList<>();  
  
        position.getAllStraightMoves(8, board(), nextPositions);  
  
        return nextPositions;  
    }  
}
```

> a) Which are the parts of the program which are poorly designed with switch statements for pieces?

The parts where it uses the `name` property to switch between different values such as `value` or `icon`. It's better to move these properties into sub-classes and use inheritance/polymorphism so that you can still use those classes in place of a `Piece`.

> b) In what ways is your new code easier to maintain than previously? What drawbacks have arisen from this new approach?

It's better in almost every way. There are minimal drawbacks, although there is now more boilerplate to add a piece. 
### 32
> In the second of the shape drawing examples in lectures, we relied on the existence of an `instanceof` operator that allowed us to check which class the object really was. The built-in ability to do this is called reflection. However, not all OOP languages support reflection. Show how we could modify the shape classes to allow us to determine the true type without using reflection.

All you would need to do is add some property on the subclass that allows you to distinguish it from other shapes, such as a `name`. The `Square` class would have `"square"` name and and so on. Then you could compare the true type to all possible name properties to determine the type.
### 33
> Explain the differences between a class, an abstract class and an interface in Java.

A class is a blueprint for an object that can be instantiated with data for the properties. 
An abstract class cannot be instantiated, but can be used to represent a parent-class with state and default methods. Sub-classes can inherit and provide alternative implementations and use the state defined on the parent class.
An interface only declares the methods that a class should have and their signatures. 
### 34
> An alternative implementation of a list to a linked list uses an array as the underlying data structure rather than a linked list.

> a) Write down the asymptotic complexities of the array-based list methods.

- Push/Pop - `O(1)`
- Insert - `O(n)`
- getIndex - `O(n)`

> b) 

```java
public interface OopList<T> {  
     int length();  
     void addFirst(T element);  
     T removeFirst();  
     T get(int n);  
}
```

```java
public class ArrayedList<T> implements OopList<T> {  
    private T[] arr;  
    private int nextFreeIndex = 0;  
  
    public ArrayedList(int size) {  
        this.arr = (T[])new Object[size];  
    }  
  
    public ArrayedList() {  
        this.arr = (T[])new Object[1];  
    }  
  
    public void addFirst(T element) {  
        if (nextFreeIndex >= arr.length) {  
            T[] doubleSizeArr = Arrays.copyOf(arr, arr.length * 2);  
            arr = doubleSizeArr;  
        }  
  
        // Shift elements to the right  
        for (int i = nextFreeIndex; i > 0; i--) {  
            arr[i] = arr[i - 1];  
        }  
  
        // Add the new element at the beginning  
        arr[0] = element;  
        nextFreeIndex++;  
    }  
  
    public T removeFirst() {  
        if (nextFreeIndex == 0) {  
            // Handle the case when the array is empty  
            throw new IllegalArgumentException("Array is empty");  
        }  
  
        T originalElement = arr[0];  
  
        // Shift elements to the left  
        for (int i = 0; i < nextFreeIndex - 1; i++) {  
            arr[i] = arr[i + 1];  
        }  
  
        // Set the last element to null (optional, depends on your requirements)  
        arr[nextFreeIndex - 1] = null;  
  
        nextFreeIndex--;  
  
        return originalElement;  
    }  
  
  
    public T get(int n) {  
        if (n >= nextFreeIndex) throw new NoSuchElementException();  
  
        return arr[n];  
    }  
  
    public int length() {  
        return nextFreeIndex;  
    }  
  
    @Override  
    public String toString() {  
        if (arr.length == 0) {  
            return "[]";  
        }  
  
        StringBuilder result = new StringBuilder("[");  
        boolean isFirstNonNullElement = true;  
  
        for (T element : arr) {  
            if (element != null) {  
                if (!isFirstNonNullElement) {  
                    result.append(", ");  
                }  
                result.append(element);  
                isFirstNonNullElement = false;  
            }  
        }  
  
        result.append("]");  
  
        return result.toString();  
    }  
  
    public static <R> ArrayedList<R> create(R[] elements) {  
        ArrayedList<R> ll = new ArrayedList<>(elements.length);  
  
        for (int i = elements.length - 1; i >= 0; i--) {  
            ll.addFirst(elements[i]);  
        }  
  
        return ll;  
    }  
}
```

> c) When adding items to an array-based list, rather than expanding the array by one each time, the array size is often doubled whenever expansion is required. Analyse this approach to get the asymptotic complexities associated with an insertion.

The complexities of this approach depend on how the arrays are implemented. An entirely new array will need to be created that is double the size, and then all items from the original half-sized array copied over, leading to a linear time complexity. They may be copied one space to the right to leave an empty space at the start of the array for the item to be inserted.
### 35
> a) Complete parts 4 and 5 of the ‘Classic collections’ (queues) task on Chime.

```java
public void reverse() {  
  Node<T> current = head;  
  Node<T> prev = null;  
  Node<T> next = null;  
  
  while (current != null) {  
    next = current.next;
    current.next = prev;

    current = next;  
  }  
  
  head = prev;  
}
```

```java
public interface OopQueue<T> {  
    public void push(T element);  
    public T pop();  
}
```

```java
public class LinkQueue<T> implements OopQueue<T> {  
    private LinkList<T> front;  
    private LinkList<T> back;  
  
    LinkQueue() {  
        front = new LinkList<>();  
        back = new LinkList<>();  
    }  
  
    @Override  
    public void push(T element) {  
        back.addFirst(element);  
    }  
  
    @Override  
    public T pop() {  
        if (front.length() == 0) {  
            if (back.length() == 0) throw new NoSuchElementException();  
  
            back.reverse();  
            front = back;  
            back = new LinkList<>();  
        }  
  
        return front.removeFirst();  
    }  
}
```

> b) State the asymptotic complexities of the approach.

`push` has a constant time complexity.
`pop` can have a constant time complexity if the front list is not empty, but otherwise the back list needs to be reversed and copied which has a linear time complexity.
### 36
> Imagine you have two classes: `Employee` (which embodies being an employee) and `Ninja `(which embodies being a Ninja). You need to represent an employee who is also a ninja (a common problem in the real world). By creating only one interface and only one class (NinjaEmployee), show how you can do this without having to copy method implementation code from either of the original classes.

You might create an interface that declares methods common to all three types of people, and then on the `NinjaEmployee` class have private properties that are instances of the two other types. Then just call the methods from both of those in each of the `NinjaEmployee` method.

```java
interface HasJob {
	public void doWork();
	// ...
}

// class Ninja implements HasJob, has doWork() method for ninja work

// class Employee implements HasJob, has doWork() method for employee work

class NinjaEmployee implements HasJob {
	private Ninja ninja;
	private Employee employee;

	public void doWork() {
		ninja.doWork();
		employee.doWork();
	}

	// and so on...
}```
### 37
> Write a small Java program that demonstrates constructor chaining using a hierarchy of three classes as follows: `A` is the parent of `B` which is the parent of `C`. Modify your definition of `A` so that it has exactly one constructor that takes an argument, and show how `B` and/or `C` must be changed to work with it.

```java
class A {
	A() {}
}

class B extends A {
	B() {}
}

class C extends B {
	C() {}
}

// to...

class A {
	A(int param) {}
}

class B extends A {
	B(int param) {
		super(param);
	}
}

class C extends B {
	C(int param) {
		super(param);
	}
}```
### 38
> The lectures described the main mechanism to mark objects that can be deleted (or to mark objects that cannot be deleted), but not the deletion process. Compare and contrast the following approaches in terms of performance. Your answer should consider the costs associated with marking and deleting.

> a) The mark-sweep schemes delete the marked objects, leaving their memory available. A list of free memory chunks is maintained.

This may have performance costs because when a new item is created, a memory space large enough to store it must be found. This may involve searching through the list of free memory chunks linearly. However, the marking and sweeping processes would be very fast and not add much overhead to the program runtime.

> b) The mark-sweep-compact schemes delete the objects and then compact the memory by moving surviving objects to be adjacent to each other (i.e. no free memory between them).

This approach adds additional time to the garbage collection process, as once the marking and sweeping as been performed the compact process may take some time to move all the remaining objects around. Depending on how many there are, this could take a long time. Afterwards, though, there would be more contiguous free memory so it would be faster to create new objects in memory, even if they need a lot of space.

> c) The mark-copy schemes maintain two memory regions, one of which is active. During the marking process, surviving objects are copied into the other region. Once marking is complete, references are updated to the second region, which becomes active, and the first region is deleted.

The copying process may take some amount of time which adds performance overhead to the garbage collection process, but this is likely to perform better than the mark-sweep-compact method because the collector could just keep track of the next free space in the second region, instead of jumping back and forth over surviving and non-surviving blocks if it was working in the same region. This also has the benefit of new object creation being faster because the second region would not be fragmented like in mark-sweep.
### 39
> It is often recommended that we make classes immutable wherever possible. How would you expect this to impact garbage collection?

Immutable classes and objects often lead to a paradigm where *new* copies of objects are made with slight changes, rather than mutating the original object. This means overall there would likely be more objects in memory during the program's runtime, which may mean garbage collection takes longer. However, often once one instance of an immutable class has been used to create a new one slightly changed, the original is not used again so there might be more object deletion than when mostly using mutable classes.
### 40
> What are the issues with using finalisers?

The finaliser method runs just before the Java garbage collector deletes an object. There are some issues with its use, including:
- There are better, more specialised ways to control the closing/releasing of resources
- It is difficult to predict when exactly the JVM will call the finaliser method.
- It can have a performance impact, or delay when objects are deleted and memory reclaimed.
### 41
> In Java try...finally blocks can be applied to any code—no catch is needed. The code in the finally block is guaranteed to run after that in the try block. Suggest how you could make use of this to emulate the behaviour of a destructor (which is called immediately when indicate we are finished with the object, not at some indeterminate time later).

You could wrap all of your code that uses some instance of an object in a `try` block. Once you are sure you no longer need the object, close the `try` block and add a `finally` block, in which you could add destructor code - you might call methods on the object to clean up resources used, close files or streams, etc. This `finally` block will *always* run after the `try` block, even if an exception is thrown, so it can act as a destructor for the object as the object will never be used again after `try` block, regardless of the result.
### 42
> By experimentation or otherwise, work out what happens when the following method is executed.![[Pasted image 20231121145349.png]]

Whilst the `try` block has a `return` statement for the method, the `finally` block will *still* be run before the method returns to the previous program location. 
### 43
> What would you say are the three most important concepts from the content for this supervision?

- Abstract classes and interfaces
- How polymorphism can be used to design class hierarchies and programs
- How Java cleans up unused memory
### 44
> Give one question that you would like to discuss in the supervision.

How exactly do `finally` blocks work.