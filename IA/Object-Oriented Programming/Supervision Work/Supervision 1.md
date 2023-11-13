**Name:** Brace Godfrey
**CRSId:** dg681
### 1
> Set up SSH, git, and IDE and your account on Chime.

Done :)
### 2
> Complete the Fibonacci task on Chime.

```java
class Fibonacci {  
	int fib(int i) {  
	    if (i < 0) throw new IllegalArgumentException();  
	    if (i <= 1) return i;  
	  
	    return fib(i - 1) + fib(i - 2);  
    }  
}
```

```java
class FibonacciTable {  
    private final Map<Integer, Integer> cache;  
    
    FibonacciTable() {  
        this(new HashMap<>());  
    }  
  
    FibonacciTable(Map<Integer, Integer> cache) {  
        this.cache = cache;  
    }  
  
    int fib(int i) {  
        if (i < 0) throw new IllegalArgumentException();  
        if (cache.containsKey(i)) return cache.get(i);  
        if (i <= 1) return i;  
  
        int value = fib(i - 1) + fib(i - 2);  
        cache.put(i, value);  
        return value;  
    }  
}
```

```java
public class FibonacciTest {  
    @Test  
    public void fibonacci_returns1_for1() {  
        // ARRANGE  
        Fibonacci fibonacci = new Fibonacci();  
  
        // ACT  
        int result = fibonacci.fib(1);  
  
        // ASSERT  
        assertThat(result).isEqualTo(1);  
    }  
  
    @Test  
    public void fib_throwsIllegalArgumentException_atMinus1() {  
        // ARRANGE  
        Fibonacci fibonacci = new Fibonacci();  
  
        // ACT  
        // ASSERT
        assertThrows(IllegalArgumentException.class, () -> fibonacci.fib(-1));  
    }  
  
    @Test  
    public void fib_returns0_for0() {  
        // ARRANGE  
        Fibonacci fibonacci = new Fibonacci();  
  
        // ACT  
        int result = fibonacci.fib(0);  
  
        // ASSERT  
        assertThat(result).isEqualTo(0);  
    }  
  
    @Test  
    public void fib_returns13_for7() {  
        // ARRANGE  
        Fibonacci fibonacci = new Fibonacci();  
  
        // ACT  
        int result = fibonacci.fib(7);  
  
        // ASSERT  
        assertThat(result).isEqualTo(13);  
    }  
}
```

```java
public class FibonacciTableTest {  
  
    @Test  
    public void fib_throwsIllegalArgumentException_atMinus1() {  
        // ARRANGE  
        FibonacciTable fibonacciTable = new FibonacciTable();  
  
        // ACT  
        // ASSERT        
        assertThrows(IllegalArgumentException.class, () -> fibonacciTable.fib(-1));  
    }  
  
    @Test  
    public void fibonacci_returns1_for1() {  
        // ARRANGE  
        FibonacciTable fibonacciTable = new FibonacciTable();  
  
        // ACT  
        int result = fibonacciTable.fib(1);  
  
        // ASSERT  
        assertThat(result).isEqualTo(1);  
    }  
  
    @Test  
    public void fib_makesUseOfCache() {  
        // ARRANGE  
        CountingMap countingMap = new CountingMap();  
        FibonacciTable fibonacciTable = new FibonacciTable(countingMap);  
  
        // ACT  
        int result = fibonacciTable.fib(7);  
  
        // ASSERT  
        assertThat(result).isEqualTo(13);  
        assertThat(countingMap.getCounter()).isEqualTo(4);  
    }  
}
```

> (a) Why would one argue that the provided tests for Fibonacci are not sufficient?

Because they do not cover enough input cases for all of the lines in the Fibonacci implementation to be run - they only cover the base case. Additional tests were required to test the exception throwing line and the recursive formula line.

> (b) Why would it be a bad idea to write a test that times whether FibonacciTable runs faster than Fibonacci?

Code tests that rely on timing can be inconsistent depending on factors like hardware differences and system loads. The result of the test wouldn't necessarily indicate whether or not each implementation is working correctly, as tests are designed to do.
### 3
> What does instruction coverage mean as a test coverage metric? Give an example showing how 100% instruction coverage does not mean your program is bug free.

Instruction coverage is a percentage that indicates how many instructions or commands were run throughout the course of the test harness. Since every line of code in a program is for a specific purpose, even ones that throw errors or provide recursion base cases, every instruction should be tested at least once.
However, 100% instruction coverage does not mean a program is free from bugs, usually because the actual program failed to implement a certain operation or check. If the program takes input from the user as a string, and coverts this to an integer to perform operations on it, a programmer could write tests that cover all instructions in that function, but it is not necessarily bug-free because the function is missing validation checks against the input. The program might crash if the user types in something that cannot be converted to a string as the function does not check for this.
### 4
> Compare and contrast a typical functional language and a typical imperative language.

An imperative programming language tends to create programs that consist of sequential commands that need to be executed in order with some number of inputs to achieve a result, or perform side effects. It may have control structures such as if-else gates, while and for loops or procedures for the programmer to describe how exactly the desired result should be achieved. Mutable variables are also used, 
A functional programming language creates programs that describe what the result should be, rather than how to get there. It tends to only have immutable variables, and pure functions that always return the same output given the same input. Pattern matching, recursion and higher order functions are all common patterns used to achieve a functional programming style.
Functional programming languages may have some level of imperative features and vice versa, these are known as hybrid or multi-paradigm languages.
### 5
> Write Java code to test whether your Java environment performs tail-recursion optimisations or not

```java
boolean testTailRecursion(int i) {  
    if (i == 0) return true;  
  
    return testTailRecursion(i - 1);  
}

testTailRecursion(1000000000);
```

The above code results in a `StackOverflowError` even though after each recursive call the current stack frame is no longer needed and can be safely discarded, showing that the Java environment does *not* perform tail-recursion optimisations.
### 6
> Write a static function `lowestCommon` that takes two `long` arguments and returns the position of the first set bit in common, where position `0` is the LSB. If there is no common bit, the function should return `-1`.
> For example `lowestCommon(14,25)` would be `3`. Your solution should use at least one `break` statement.

```java
static int testCommon(long i, long j) {  
    int counter = 0;  
  
    while (i > 0 && j > 0) {  
        if ((i & j) % 2 != 0) break;  
  
        i = i >> 1;  
        j = j >> 1;  
        counter++;  
    }  
  
    return counter;  
}
```
### 7
> Identify the primitives, references, classes and objects in the following Java code:
> ![[Pasted image 20231112204358.png]]

Primitives:
- `d`
- `5.0`
- `1`, `2`, `3`, `4`
- `f`
- `null`

References:
- `i[]`
- `l`
- `k`
- `t`
- `c`

Classes:
- `LinkedList`
- `Double`
- `Tree`
- `Computer`

Objects:
- `{1,2,3,4}`
- `new LinkedList<Double>()`
- `new Double()`
### 8
> You met the idea of linked lists in FoCS. Complete the first part of the ‘Classic collections’ task on Chime.

```java
public class LinkList {  
  private static class Node {  
    private int value;  
    private Node next;  
  
    public Node(int value, Node next) {  
      this.value = value;  
      this.next = next;  
    }  
  
    Node(int value) {  
      this.value = value;  
      this.next = null;  
    }  
  
    @Override  
    public String toString() {  
      if (next == null) {  
        return String.valueOf(value);  
      }  
      return value + "," + next;  
    }  
  }  
  
  private Node head;  
  
  LinkList() {  
    this.head = null;  
  }  
  
  void addFirst(int element) {  
    if (head == null) {  
      head = new Node(element);  
    } else {  
      head = new Node(element, head);  
    }  
  }  
  
  int removeFirst() {  
    if (head == null) throw new NoSuchElementException();  
  
    int temp = head.value;  
    head = head.next;  
  
    return temp;  
  }  
  
  int get(int n) {  
    Node curr = head;  
    if (curr == null) throw new NoSuchElementException();  
  
    for (int i = 0; i < n; i++) {  
      if (curr == null) throw new NoSuchElementException();  
  
      curr = curr.next;  
    }  
  
    return curr.value;  
  }  
  
  int length() {  
    int counter = 0;  
    Node curr = head;  
  
    while (curr != null) {  
      counter++;  
      curr = curr.next;  
    }  
  
    return counter;  
  }  
  
  @Override  
  public String toString() {  
    return String.format("[%s]", head == null ? "" : head.toString());  
  }  
  
  public static LinkList create(int[] elements) {  
    LinkList ll = new LinkList();  
  
    for (int i = elements.length - 1; i >= 0; i--) {  
      ll.addFirst(elements[i]);  
    }  
  
    return ll;  
  }  
}
```

```java
@RunWith(JUnit4.class)  
public class LinkListTest {  
  
  @Test  
  public void linkList_toStringIsEmptyList_whenListEmpty() {  
    // ARRANGE  
    LinkList empty = new LinkList();  
  
    // ACT  
    String value = empty.toString();  
  
    // ASSERT  
    assertThat(value).isEqualTo("[]");  
  }  
  
  @Test  
  public void linkList_toStringIsSingleItem_whenListContainsOneItem() {  
    // ARRANGE  
    LinkList list = new LinkList();  
    list.addFirst(1);  
  
    // ACT  
    String value = list.toString();  
  
    // ASSERT  
    assertThat(value).isEqualTo("[1]");  
  }  
  
  @Test  
  public void linkList_toStringIsTwoThenOne_whenListHasOneThenTwoAdded() {  
    // ARRANGE  
    LinkList list = new LinkList();  
    list.addFirst(1);  
    list.addFirst(2);  
  
    // ACT  
    String value = list.toString();  
  
    // ASSERT  
    assertThat(value).isEqualTo("[2,1]");  
  }  
  
  @Test  
  public void linkList_create_fromEmptyArray() {  
    // ARRANGE  
    int[] elements = {};  
    LinkList list = LinkList.create(elements);  
  
    // ACT  
    String value = list.toString();  
  
    // ASSERT  
    assertThat(value).isEqualTo("[]");  
  }  
  
  @Test  
  public void linkList_create_fromNonEmptyArray() {  
    // ARRANGE  
    int[] elements = {1,2,3,4};  
    LinkList list = LinkList.create(elements);  
  
    // ACT  
    String value = list.toString();  
  
    // ASSERT  
    assertThat(value).isEqualTo("[1,2,3,4]");  
  }  
  
  @Test(expected = NoSuchElementException.class)  
  public void linkList_removeFirst_empty() {  
    // ARRANGE  
    LinkList list = new LinkList();  
  
    // ACT  
    list.removeFirst();  
  }  
  
  @Test  
  public void linkList_removeFirst_nonEmpty() {  
    // ARRANGE  
    int[] elements = {1,2,3,4};  
    LinkList list = LinkList.create(elements);  
  
    // ACT  
    int value = list.removeFirst();  
  
    // ASSERT  
    assertThat(list.toString()).isEqualTo("[2,3,4]");  
    assertThat(value).isEqualTo(1);  
  }  
  
  @Test(expected = NoSuchElementException.class)  
  public void linkList_get_empty() {  
    // ARRANGE  
    LinkList list = new LinkList();  
  
    // ACT  
    list.get(2);  
  }  
  
  @Test  
  public void linkList_get_nonEmpty() {  
    // ARRANGE  
    int[] elements = {1,2,3,4};  
    LinkList list = LinkList.create(elements);  
  
    // ACT  
    // ASSERT    
    assertThat(list.get(2)).isEqualTo(3);  
  }  
  
  @Test  
  public void linkList_length_zero() {  
    // ARRANGE  
    LinkList list = new LinkList();  
  
    // ACT  
    // ASSERT    
    assertThat(list.length()).isEqualTo(0);  
  }  
  
  @Test  
  public void linkList_length_nonZero() {  
    // ARRANGE  
    int[] elements = {1,2,3,4};  
    LinkList list = LinkList.create(elements);  
  
    // ACT  
    // ASSERT    
    assertThat(list.length()).isEqualTo(4);  
  }  
}
```

> a) How is an empty list represented?

An empty list is represented by an instance of the `LinkedList` class with the `head` property set to `null`.

> b) How does the `toString` method implement the list traversal?

The `toString` method implements list traversal by starting with the `head` node and calling the node's `toString` method, which recursively adds its own value to string as well as the value of the next node. When the next node is `null` all nodes have been visited and the string is formatted in the `LinkedList` method into a result that looks like an array.

> c) Given that you are making use of the addFirst method why would it be considered better style to use a static factory method to construct a list rather than doing all the work in the constructor.

Because the `addFirst` method belongs to an instance of the class, to use it in the constructor the constructor would have to create an empty instance of the `LinkList` class, then add elements using `addFirst` then return that instance, which is bad style as constructors already have an implicit object instance associated with them.

> d) Give the UML class diagram for your code.
![[Pasted image 20231112214716.png]]
### 9
> In mathematics, a set of integers refers to a collection of integers that contains no duplicates. You typically want to insert numbers into a set and query whether the set contains numbers. One approach is to store the numbers in a binary search tree.

> a) The diagram below represents this approach. Implement it in Java, using the names of entities to decide what they should do. Provide appropriate tests using JUnit. ![[Pasted image 20231112215205.png]]

```java
public class SearchSet {  
    private int mElements;  
    private BinaryTreeNode mHead;  
  
    SearchSet() {  
        mElements = 0;  
        mHead = null;  
    }  
  
    public static void main(String[] args) {}  
  
    public int getNumberElements() {  
        return mElements;  
    }  
  
    public boolean contains(int value) {  
        BinaryTreeNode current = mHead;  
  
        while (current != null) {  
            if (value == current.getValue()) {  
                return true;  
            } else if (value < current.getValue()) {  
                current = current.getLeft();  
            } else {  
                current = current.getRight();  
            }  
        }  
  
        return false;  
    }  
  
    public void insert(int i) {  
        BinaryTreeNode newNode = new BinaryTreeNode(i);  
  
        if (mHead == null) {  
            mHead = newNode;  
            return;  
        }  
  
        BinaryTreeNode current = mHead;  
        while (true) {  
            if (i < current.getValue()) {  
  
                if (current.getLeft() == null) {  
                    current.setLeft(newNode);  
                    return;  
                } else {  
                    current = current.getLeft();  
                }  
  
            } else if (i > current.getValue()) {  
  
                if (current.getRight() == null) {  
                    current.setRight(newNode);  
                    return;  
                } else {  
                    current = current.getRight();  
                }  
  
            } else {  
                throw new Error("Value already exists in set!");  
            }  
        }  
    }  
}
```

```java
public class BinaryTreeNode {  
    private int mValue;  
    private BinaryTreeNode mLeft;  
    private BinaryTreeNode mRight;  
  
    BinaryTreeNode(int value) {  
        mValue = value;  
    }  
  
    public int getValue() {  
        return mValue;  
    }  
  
    public void setValue(int value) {  
        mValue = value;  
    }  
  
    public BinaryTreeNode getLeft() {  
        return mLeft;  
    }  
  
    public void setLeft(BinaryTreeNode left) {  
        mLeft = left;  
    }  
  
    public BinaryTreeNode getRight() {  
        return mRight;  
    }  
  
    public void setRight(BinaryTreeNode right) {  
        mRight = right;  
    }  
}
```

> b) The `BinaryTreeNode` class can be reused for other solutions. Create a class `FunctionalArray` that uses `BinaryTreeNode` to create a functional array of `int`s. Your class should have a constructor that creates a tree of a given size (passed as an argument); a `void set(int index, int value)` method; and a `int get(int index)` method. You should make the functional array zero-indexed to match java’s normal arrays (i.e. the first element has index `0`). Requests for indices outside the limits should result in an exception.

```java
public class FunctionalArray {  
    private BinaryTreeNode head;  
    private int size;  
  
    FunctionalArray(int nSize) {  
        head = null;  
        size = nSize;  
  
        for (int i = 0; i < size; i++) {  
            set(i, i);  
        }  
    }  
  
    public void set(int index, int value) {  
        if (index > (size - 1)) throw new IllegalArgumentException();  
  
        if (head == null) {  
            head = new BinaryTreeNode(value);  
            return;  
        }  
  
        BinaryTreeNode curr = head;  
  
        while (index > 0) {  
            index = index / 2;  
  
            if (index % 2 == 0) {  
                if (curr.getLeft() == null) {  
                    curr.setLeft(new BinaryTreeNode(value));  
                }  
  
                curr = curr.getLeft();  
            } else {  
                if (curr.getRight() == null) {  
                    curr.setRight(new BinaryTreeNode(value));  
                }  
  
                curr = curr.getRight();  
            }  
        }  
  
        curr.setValue(value);  
    }  
  
  
    public void inorder() {  
        inorderRec(head);  
    }  
  
    private void inorderRec(BinaryTreeNode root) {  
        if (root != null) {  
            inorderRec(root.getLeft());  
            System.out.print(root.getValue() + " ");  
            inorderRec(root.getRight());  
        }  
    }  
  
    public int get(int index) {  
        if (index > (size - 1)) throw new IllegalArgumentException();  
  
        BinaryTreeNode curr = head;  
  
        while (index > 0) {  
            if (index % 2 == 0) {  
                curr = head.getLeft();  
            } else {  
                curr = head.getRight();  
            }  
  
            index = index >> 1;  
        }  
  
        return curr.getValue();  
    }  
}
```
### 10
> Explain why this code prints 0 rather than 7. ![[Pasted image 20231112215427.png]]

The `public void Test()` method on the `Test` class is not acting as an constructor because it has a explicit return type of `void`. This makes it just a method on the class that can be called on instantiated objects, but not used to create an instance. Instead a default value of `0` is used when the class declares its field `x`, and this is what `x` is initialised as when `t` is created.
### 11
Benefits:
- You can control access to particular data in a class, to reduce potential misuse of the data.
- You can change internal implementations without changing the external interface presented by the class.
- You can implement validation checks in setter methods to ensure the data is always valid.
Drawbacks:
- It can come with a load of boilerplate code that could look messy or hard to read if not properly named and structured.
### 12
Advantages over Java: 
- Less boilerplate, much easier to see at a glance what state is public or private, and less functions needed on the class to get/set state.
- This also means python is easier to prototype with, which can be useful for fast-moving development or smaller scripts that do not need to be particularly robust or safe.
- Less opportunity to validate state when using public variables.
Disadvantages over Java:
- Relies on programmers knowing and understanding the convention, could lead to confusion for new programmers or ones who have not encountered it before.
- Higher chance to accidentally set invalid or dangerous state.
- More opportunity to intentionally misuse public state to use program in an unintended way.
- Overall less suitable when the program needs to be very robust and safe for critical systems, as a higher chance of bugs or exploits involving state.
### 13
> Complete the Matrices programming task on Chime.

```java
class Matrix {  
  private final double[][] elements;  
  private final int width;  
  private final int height;  
  
  private Matrix(int height, int width) {  
    this(new double[height][width]);  
  }  
  
  /** Create a new matrix based on the elements provided. */  
  Matrix(double[][] elements) {  
    this.width = elements[0].length;  
    this.height = elements.length;  
  
    var els = new double[height][width];  
  
    for (int i = 0; i < height; i++) {  
      els[i] = elements[i].clone();  
    }  
  
    this.elements = els;  
  }  
  
  /** Multiply this matrix by the provided matrix and return the result. */  
  Matrix mult(Matrix other) {  
    if (width != other.height) throw new IllegalArgumentException();  
  
    Matrix result = new Matrix(height, other.width);  
  
    for (int i = 0; i < height; i++) {  
      for (int j = 0; j < other.width; j++) {  
        double sum = 0;  
  
        for (int k = 0; k < width; k++) {  
          sum += elements[i][k] * other.elements[k][j];  
        }  
  
        result.elements[i][j] = sum;  
      }  
    }  
  
    return result;  
  }  
  
  /** Add this matrix to the provided matrix and return the result. */  
  Matrix add(Matrix other) {  
    if (width != other.width || height != other.height) {  
      throw new IllegalArgumentException("Dimension mismatch");  
    }  
  
    Matrix r = new Matrix(height, width);  
    for (int col = 0; col < width; col++) {  
      for (int row = 0; row < height; row++) {  
        r.elements[row][col] = elements[row][col] + other.elements[row][col];  
      }  
    }  
    return r;  
  }  
  
  /** Transpose this matrix and return the result. */  
  Matrix transpose() {  
    Matrix result = new Matrix(width, height);  
  
    for (int row = 0; row < height; row++) {  
      for (int col = 0; col < width; col++) {  
        result.elements[col][row] = elements[row][col];  
      }  
    }  
  
    return result;  
  }  
  

  double get(int row, int col) {  
    return elements[row][col];  
  }  
  
  int width() {  
    return width;  
  }  
  
  int height() {  
    return height;  
  }  
  
  @Override  
  public String toString() {  
    return Arrays.deepToString(elements);  
  }  
}
```

```java
class Shapes {  
  static Matrix rotation2d(double degrees) {  
    double[][] data = new double[2][2];  
    double rads = Math.toRadians(degrees);  
  
    data[0][0] = Math.cos(rads);  
    data[0][1] = -Math.sin(rads);  
    data[1][0] = Math.sin(rads);  
    data[1][1] = Math.cos(rads);  
  
    return new Matrix(data);  
  }  
  
  /** Create a new identity matrix with the specified size. */  
  static Matrix identity(int size) {  
    double[][] data = new double[size][size];  
    for (int i = 0; i < size; i++) {  
      data[i][i] = 1;  
    }  
    return new Matrix(data);  
  }  
  
  public static Matrix square(int size) {  
    if (size < 0) throw new IllegalArgumentException("Size must be non-negative");  
  
    double[][] squarePoints = new double[2][8 * size];  
    int index = 0;  
  
    // bottom  
    for (int i = -size; i <= size; i++) {  
      squarePoints[0][index] = i;  
      squarePoints[1][index] = -size;  
      index++;  
    }  
  
    // right  
    for (int i = -size + 1; i <= size; i++) {  
      squarePoints[0][index] = size;  
      squarePoints[1][index] = i;  
      index++;  
    }  
  
    // top  
    for (int i = size - 1; i >= -size; i--) {  
      squarePoints[0][index] = i;  
      squarePoints[1][index] = size;  
      index++;  
    }  
  
    // left  
    for (int i = size - 1; i > -size; i--) {  
      squarePoints[0][index] = -size;  
      squarePoints[1][index] = i;  
      index++;  
    }  
  
    return new Matrix(squarePoints);  
  }  
  
  // No instances  
  private Shapes() {}  
}
```

> a) Why is it necessary for the assertions to have the form `assertThat(actual).isWithin(tolerance).of(expected);`?

This is necessary because of floating point precision errors when working with decimal numbers (in this case, `double`s) in Java. The tolerance is a small value, and the test checks if the result is within that tolerance of the expected result.

> b) Why have the static factory methods been put in the `Shapes` class rather than `Matrix`?

The factory methods have been put in the `Shapes` class because of the single responsibility principle - the primary purpose of `Matrix` is to represent one matrix as well as operations that can be performed on it/with other matrices.
The primary purpose of the `Shapes` class is to hold specific instances of `Matrix` that can be used for shape and point manipulation. This is a different responsibility to the `Matrix` class and these should be stored separately for modularity and reusability, and to avoid cluttering the `Matrix` class.
### 14
> This question considers representing a 2D vector in Java.

> a) Develop a mutable class Vector2D to embody the notion of a 2D vector based on floats (do not use Generics). At a minimum your class should support addition of two vectors; scalar product; normalisation and magnitude.

```java
public class Vector2D {  
    private double x = 0;  
    private double y = 0;  
  
    Vector2D(double x, double y) {  
        this.x = x;  
        this.y = y;  
    }  
  
    Vector2D(double c) {  
        this.x = c;  
        this.y = c;  
    }  
  
    public void scalar(Vector2D a) {  
        x *= a.x;  
        y *= a.y;  
    }  
  
    public void normalise() {  
        double m = magnitude();  
  
        x /= m;  
        y /= m;  
    }  
  
    public double magnitude() {  
        return Math.sqrt(Math.pow(x, 2) + Math.pow(y, 2));  
    }  
}
```

> b) What changes would be needed to make it immutable?

The `x` and `y` properties could be set to `final` and only set once in the constructor. Each method should return `Vector2D` instead of void, and would have to initialise a `new Vector2D` in their body to act as the result of the calculation. The fields of this vector can be set to the result of each operation and this vector returned, leaving the instance vector and the parameter vector untouched.

> c) Contrast the following prototypes for the addition method for both the (i) mutable, and (ii) immutable versions.![[Pasted image 20231112235017.png]]

The first prototype would only be useful for the mutable version, as there is no way to return the result of the calculation without mutating either the instance or the parameter.

The second one could be used for an immutable or mutable implementation but makes the most sense for the immutable. The instance is one of the vectors to be added and the parameter is the other, and then the function returns a new vector that is the result. 
However, this could also be used for the mutable implementation as it would allow you to "chain" functions together if the return value of the function is the instance object after the addition with the parameter.

The third one is a bad implementation because it requires an instance to be used, even though two vectors are being passed in. It may mutate the original instance although this is unclear. 

The fourth one makes more sense because it is a static method on the `Vector2D` class, indicating it is an operation on the class. It takes two vectors and assumedly returns the result. It is very unlikely to mutate either of the two parameters vectors, and because it is static it has no instance which it could mutate either. It is likely an immutable implementation.

> d) How can you convey to a user of your class that it is immutable?

You could present this clearly in your documentation (either code comments or externally), declaring fields as "final" to indicate they are never changed (although this could lead to problems the fields being references and the user mutating the actual data they are pointing to), and ensuring that there are no mutating methods on your class or labelld as such.
### 15
> Explain in detail why Java’s Generics do not support the use of primitive types as the parameterised type? Why can you not instantiate objects of the template type in generics (i.e. why is `new T()` forbidden?)

I am not entirely sure why Java doesn't allow you to use primitive types to fill a generic type.
I imagine you cannot instantiate objects of the template type because the compiler loses some information about the datatype?
### 16
> Complete the second part of the ‘Classic collections’ task (using generics) on Chime.

```java
public class LinkList<T> {  
  private static class Node<N> {  
    private N value;  
    private Node<N> next;  
  
    public Node(N value, Node<N> next) {  
      this.value = value;  
      this.next = next;  
    }  
  
    Node(N value) {  
      this.value = value;  
      this.next = null;  
    }  
  
    @Override  
    public String toString() {  
      if (next == null) {  
        return String.valueOf(value);  
      }  
      return value + "," + next;  
    }  
  }  
  
  private Node<T> head;  
  
  LinkList() {  
    this.head = null;  
  }  
  
  void addFirst(T element) {  
    if (head == null) {  
      head = new Node<>(element);  
    } else {  
      head = new Node<>(element, head);  
    }  
  }  
  
  T removeFirst() {  
    if (head == null) throw new NoSuchElementException();  
  
    T temp = head.value;  
    head = head.next;  
  
    return temp;  
  }  
  
  T get(int n) {  
    Node<T> curr = head;  
    if (curr == null) throw new NoSuchElementException();  
  
    for (int i = 0; i < n; i++) {  
      if (curr == null) throw new NoSuchElementException();  
  
      curr = curr.next;  
    }  
  
    return curr.value;  
  }  
  
  int length() {  
    int counter = 0;  
    Node<T> curr = head;  
  
    while (curr != null) {  
      counter++;  
      curr = curr.next;  
    }  
  
    return counter;  
  }  
  
  @Override  
  public String toString() {  
    return String.format("[%s]", head == null ? "" : head.toString());  
  }  
  
  public static <R> LinkList<R> create(R[] elements) {  
    LinkList<R> ll = new LinkList<>();  
  
    for (int i = elements.length - 1; i >= 0; i--) {  
      ll.addFirst(elements[i]);  
    }  
  
    return ll;  
  }  
}
```
### 17
```java
public class InsertionSorter<T> implements Sorter<T> {  
  @Override  
  public void sort(T[] array, Comparator<T> comparator) {  
    int n = array.length;  
  
    for (int i = 1; i < n; ++i) {  
      T key = array[i];  
      int j = i - 1;  
  
      while (j >= 0 && comparator.compare(array[j], key) > 0) {  
        array[j + 1] = array[j];  
        j = j - 1;  
      }  
  
      array[j + 1] = key;  
    }  
  }  
}
```

```java
public class MergeSorter<T> implements Sorter<T> {  
  private void merge(T[] array, T[] left, T[] right, Comparator<T> comparator) {  
    int i = 0;  
    int j = 0;  
    int k = 0;  
  
    while (i < left.length && j < right.length) {  
      if (comparator.compare(left[i], right[j]) <= 0) {  
        array[k] = left[i];  
        i++;  
      } else {  
        array[k] = right[j];  
        j++;  
      }  
      k++;  
    }  
  
    while (i < left.length) {  
      array[k] = left[i];  
      i++;  
      k++;  
    }  
  
    while (j < right.length) {  
      array[k] = right[j];  
      j++;  
      k++;  
    }  
  }  
  
  @Override  
  public void sort(T[] array, Comparator<T> comparator) {  
    int n = array.length;  
  
    if (n > 1) {  
        int mid = n / 2;  
  
        T[] left = Arrays.copyOfRange(array, 0, mid);  
        T[] right = Arrays.copyOfRange(array, mid, n);  
  
        sort(left, comparator);  
        sort(right, comparator);  
  
        merge(array, left, right, comparator);  
      }  
    }  
}
```

Unsure how to do quicksort in-place
### 18S
> Research the notion of *wildcards* in Java Generics. Using examples, explain the problem they solve.

Wildcards are used to represent unknown types or a family of types. The `?` wildcard is unbounded, and represents and unknown type. It can be used as a generic when the actual value is not really cared about. 

For example, you may want to process items in a `List` without knowing their type - for example just counting how many there are. The `List` class still requires a generic type, so the `?` wildcard type can stand in as an unknown type. 

A more useful wildcard is the upper-bounded wildcard `? extends T`.  This represents a family of types that all subtypes of `T`. For example, `Integer`, `Double` or `BigDecimal` are all subtypes of `Number`. 
You would use `? extends Number` as a generic type when you have some process than can be performed on any sub-type of number, and want to keep your function general whilst avoiding excessive casting or overriding. For example, the signature of a function that sums up all items in a list might be `public static double calculateSum(List<? extends Number> numbers)`, and you could pass in a list of any subtype of `Number`.

The lower-bounded wildcard is `? super T`. This represents the type or any superclass of that type. I am unsure when exactly this might be useful.

Wildcards represent unknown types but can be bounded by a specific level of type hierarchy. This helps you write more general code with less boilerplate, whilst retaining type safety.
### 19
> Pointers are problematic because they might not point to anything useful. A null reference doesn’t point to anything useful. So what is the advantage of using references over pointers?

References are guaranteed to point either to some existing data or object that was created by the program and was/is/will be used by the program, or to nothing (`null`). This makes them much safer than pointers, as pointers can point to arbitrary memory locations, including data that was not meant to be accessed by the program. 
### 20
> Draw some simple diagrams to illustrate what happens with each step of the following Java code in memory.

![[Pasted image 20231113003317.png]]

During *3*, both `p` and `p2` have a reference to memory address `0` and are pointing to the same location.
During *4*, `p` is still pointing to `0` whereas `p2` has been updated to memory location `1`.
### 21
> What would you say are the three most important concepts from the content for this supervision?

- Generics and how to use them effectively.
- Immutability and different ways of implementing it.
- Advantages of encapsulation.
### 22
> Give one question that you would like to discuss in the supervision

What could the lower-bounded wildcards (`? super T`) be used for?