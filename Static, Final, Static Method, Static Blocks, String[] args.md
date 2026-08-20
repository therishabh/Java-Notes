# Java: `static`, `final`, Static Methods, Static Blocks & `String[] args`

---

# 1. `static` and `final` in Java

## 1.1 `static` — Actual Meaning

The simplest mental model for `static` in Java is:

> **A `static` member is associated with the class, not with any particular object/instance.**

Example:

```java
class User {
    static String applicationName = "MyApp";
    String name;
}
```

If:

```java
User u1 = new User();
User u2 = new User();

u1.name = "John";
u2.name = "Mike";
```

Then:

```text
u1.name → John
u2.name → Mike
```

because `name` is an instance field.

But:

```java
User.applicationName
```

represents only **one class-level value**.

Conceptually:

```text
User Class
   │
   └── applicationName = "MyApp"
          ↑
          │
      ┌───┴───┐
      │       │
     u1      u2
```

`u1` and `u2` do not have separate copies of `applicationName`.

---

## 1.2 Static Field / Static Variable

```java
class Counter {

    static int count = 0;

    Counter() {
        count++;
    }
}
```

Now:

```java
Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();

System.out.println(Counter.count);
```

Output:

```text
3
```

Important point:

```java
count
```

is not object-specific.

On every object creation, the same static field is being modified.

### If it were an instance field:

```java
class Counter {
    int count = 0;
}
```

Then:

```java
Counter c1 = new Counter();
Counter c2 = new Counter();

c1.count++;
```

result:

```text
c1.count = 1
c2.count = 0
```

Whereas in the static case:

```text
Counter.count = 1
```

and if you increment through the second object:

```text
Counter.count = 2
```

---

# 1.3 How should you access a static member?

Preferred approach:

```java
ClassName.member
```

Example:

```java
Counter.count
Math.PI
Integer.MAX_VALUE
```

Technically, Java also allows accessing a static member through an object reference:

```java
Counter c = new Counter();

System.out.println(c.count);
```

But **this is not recommended**.

Better:

```java
Counter.count
```

because `count` is a member of the class, not of the object.

### Interview trap

Question:

> Can static members be accessed using an object?

Answer:

**Yes, Java allows it, but it is discouraged because static members belong to the class, not the object.**

---

# 1.4 Lifecycle of a Static Field

Static members are associated with class initialization.

Simplified mental model:

```text
Class becomes initialized
        ↓
Static fields initialized
        ↓
Static blocks executed
        ↓
Class is ready to use
```

Static initialization generally happens **once, during the class initialization phase**.

This is important:

> Static initialization is associated with the class, not every object creation.

---

# 1.5 Static + Multiple Objects

```java
class Employee {

    static String company = "ABC";
    String name;
}
```

```java
Employee e1 = new Employee();
Employee e2 = new Employee();

e1.name = "John";
e2.name = "Mike";
```

Memory conceptually:

```text
Employee Class
   │
   └── static company = "ABC"

e1
 └── name = "John"

e2
 └── name = "Mike"
```

Therefore:

```java
e1.company
e2.company
Employee.company
```

all three refer to the same static field.

---

# 1.6 Static with Inheritance

Example:

```java
class Parent {
    static int value = 10;
}

class Child extends Parent {
}
```

You can write:

```java
System.out.println(Child.value);
```

But an important conceptual point:

> Static members are not polymorphic like instance methods.

In the context of inheritance, the behaviour of static members differs from that of instance members.

---

# 1.7 Static Field in Parent and Child

```java
class Parent {
    static int value = 10;
}

class Child extends Parent {
    static int value = 20;
}
```

Now:

```java
System.out.println(Parent.value);
System.out.println(Child.value);
```

Output:

```text
10
20
```

This is the concept of **field hiding**, not polymorphic overriding.

---

# 1.8 Important restrictions of `static`

Inside a static context, you cannot directly access instance members.

Example:

```java
class Demo {

    int value = 10;

    static void test() {
        System.out.println(value); // ERROR
    }
}
```

Why?

Because:

```text
static method → no specific object
instance field → belongs to an object
```

The JVM has no idea **which object's `value`** is being referred to.

Correct:

```java
static void test() {
    Demo d = new Demo();
    System.out.println(d.value);
}
```

---

# 1.9 `final` — Actual Meaning

The core meaning of `final`:

> **Once a final variable is assigned, that variable cannot be assigned another value/reference.**

But the behaviour of `final` depends on what it is applied to:

* variable
* reference
* method
* class
* parameter

---

# 1.10 Final Primitive

```java
final int age = 30;
```

This is invalid:

```java
age = 40; // ERROR
```

Because `age` cannot be reassigned.

---

# 1.11 Final Reference — Very Important

This is a favourite interview trap.

```java
final User user = new User();
```

Many beginners assume:

> The User object is immutable.

**Wrong.**

Here, `final` makes **the reference final**.

Conceptually:

```text
user
 │
 └──────────► User Object
              name = "John"
```

You cannot do:

```java
user = new User(); // ERROR
```

because you are changing the `user` reference.

But:

```java
user.name = "Mike";
```

may be possible, assuming `name` is mutable.

So:

```text
final reference
      ↓
reference cannot point somewhere else

BUT

object itself
      ↓
may still be mutable
```

### Therefore:

> **`final` does not automatically make an object immutable.**

---

# 1.12 Final Method

```java
class Parent {

    final void display() {
        System.out.println("Parent");
    }
}
```

Child class:

```java
class Child extends Parent {

    void display() {   // ERROR
    }
}
```

A `final` method cannot be overridden.

Use case:

> The parent class does not want subclasses to change the implementation of a particular method.

---

# 1.13 Final Class

```java
final class SecurityConfig {
}
```

Now:

```java
class MyConfig extends SecurityConfig {
}
```

is invalid.

A final class cannot be extended.

Classic example:

```java
String
```

`String` is a final class in Java.

---

# 1.14 Final Parameter

```java
void process(final int value) {
    value = 20; // ERROR
}
```

You cannot reassign the parameter.

For a reference:

```java
void process(final User user) {
    user.name = "Mike"; // potentially allowed
    user = new User();  // ERROR
}
```

Again, the same distinction:

```text
final reference ≠ immutable object
```

---

# 1.15 `final` vs Immutability

Remember this distinction firmly.

### `final`

Controls:

> **reassignment**

### Immutability

Controls:

> **object state modification**

Example:

```java
final User user = new User();

user.name = "John";
```

Possible if `User` is mutable.

In an immutable object:

```java
user.name = "John";
```

would not be allowed at all, because no mechanism to change the object's state has been exposed.

---

# 1.16 `static final`

Now let's combine the two:

```java
public static final int MAX_RETRIES = 3;
```

Its meaning:

* `static` → class-level
* `final` → reassignment not allowed

So:

```text
MAX_RETRIES
    ↓
one class-level value
    ↓
cannot be reassigned
```

This is commonly used for constants.

Example:

```java
class HttpConfig {

    public static final int MAX_RETRIES = 3;
    public static final int TIMEOUT = 5000;
}
```

Usage:

```java
HttpConfig.MAX_RETRIES
```

---

# 1.17 Constant Naming Convention

Java convention:

```java
MAX_RETRIES
DEFAULT_TIMEOUT
MAX_CONNECTIONS
API_VERSION
```

Generally:

```text
UPPER_CASE_WITH_UNDERSCORES
```

---

# 1.18 Compile-Time Constants

This is a slightly interview-level important distinction.

```java
static final int MAX = 10;
```

Primitive/String values that can be determined at compile time can be **compile-time constants**.

Example:

```java
static final int MAX = 10;
```

But:

```java
static final int MAX = getMax();
```

this is a normal final static field, not necessarily a compile-time constant.

Similarly:

```java
static final Integer MAX = 10;
```

Because it is a reference type, it does not behave like a primitive compile-time constant.

For interviews, remember broadly:

> `static final` means a constant-like class-level value, but every `static final` field is not necessarily a compile-time constant.

---

## 🔑 Must Remember — `static` & `final`

* `static` → class-level association.
* Instance field → object-level state.
* A static field has a single class-level state.
* Access static members preferably via `ClassName.member`.
* A static context cannot directly access instance members.
* A `final` variable cannot be reassigned.
* A `final` reference does not mean the object is immutable.
* A `final` method cannot be overridden.
* A `final` class cannot be extended.
* `static final` is commonly used for constants.
* `static` + inheritance ≠ normal runtime polymorphism.

---

# 2. Static Methods

## 2.1 What is a Static Method?

```java
class MathUtils {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call:

```java
MathUtils.add(10, 20);
```

Here, calling `add()` does not require an object.

Reason:

```text
MathUtils.add()
       ↓
class-level operation
```

not:

```text
some MathUtils object
       ↓
add()
```

---

# 2.2 Static vs Instance Method

### Static

```java
class MathUtils {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call:

```java
MathUtils.add(10, 20);
```

### Instance

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }
}
```

Call:

```java
Calculator calculator = new Calculator();

calculator.add(10, 20);
```

Difference:

|                                     | Static Method    | Instance Method   |
| ----------------------------------- | ---------------- | ----------------- |
| Belongs to                          | Class            | Object            |
| Object required                     | No               | Yes               |
| Can directly access instance fields | No               | Yes               |
| Can use `this`                      | No               | Yes               |
| Polymorphic overriding              | No               | Yes               |
| Typical call                        | `Class.method()` | `object.method()` |

---

# 2.3 What can a static method access?

A static method can directly access:

```java
static int count;

static void test() {
    count++;
}
```

But:

```java
int value;

static void test() {
    value++; // ERROR
}
```

Because `value` requires an object.

---

# 2.4 Why can't `this` be used?

`this` means:

> the current object

But invoking a static method does not require a current object at all.

Example:

```java
static void test() {
    System.out.println(this); // ERROR
}
```

Conceptually:

```text
static method
     ↓
no current object
     ↓
therefore no `this`
```

---

# 2.5 Static Methods and Inheritance

Important interview question:

> Can static methods be overridden?

**No.**

Static methods **are not overridden**, they are **hidden**.

Example:

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void show() {
        System.out.println("Child");
    }
}
```

Now:

```java
Parent.show();
Child.show();
```

Output:

```text
Parent
Child
```

This is not runtime polymorphism.

---

# 2.6 Method Hiding

Example:

```java
class Parent {
    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    static void show() {
        System.out.println("Child");
    }
}
```

```java
Parent p = new Child();

p.show();
```

Output:

```text
Parent
```

Had it been an instance method, the runtime object would decide:

```text
reference type → Parent
actual object → Child
```

and the overridden instance method → `Child`

But for a static method:

> Method resolution happens based on the reference/class type, not based on runtime object polymorphism.

---

# 2.7 When should you use static methods?

Good use cases:

### Utility functions

```java
Math.max()
Math.min()
```

### Stateless operations

```java
StringUtils.isBlank(...)
```

### Factory/helper methods

```java
User.createDefaultUser()
```

### Constants-related operations

```java
Config.getDefaultTimeout()
```

But making every method static is not good design.

If an operation needs object state, an instance method is more appropriate.

---

## 🔑 Must Remember — Static Methods

* A static method is class-level.
* You can call it without creating an object.
* A static method cannot directly access instance fields/methods.
* `this` is not available in a static method.
* Static methods are not overridden.
* A static method can be **hidden** in inheritance.
* A static method does not participate in runtime polymorphism.
* Static is useful for stateless utility operations.

---

# 3. Static Blocks

The static block is a particularly important concept in Java.

Syntax:

```java
class Demo {

    static {
        System.out.println("Static block");
    }
}
```

---

# 3.1 What is a Static Block?

A static block is an initialization block that **executes at the time of class initialization**.

Example:

```java
class Demo {

    static int value = 10;

    static {
        System.out.println("Static block");
        value = 20;
    }
}
```

The primary purpose of a static block:

> **To perform complex static initialization logic.**

---

# 3.2 When does a Static Block execute?

Important:

> A static block does not execute on object creation.

It executes when the class is **initialized by the JVM**.

Simplified:

```text
Class initialization
       ↓
Static field initialization
       ↓
Static blocks
       ↓
Class ready
```

Generally, static initialization happens **once** per class initialization.

---

# 3.3 Example

```java
class Demo {

    static int value = 10;

    static {
        System.out.println("Static block");
        value = 20;
    }

    Demo() {
        System.out.println("Constructor");
    }
}
```

Then:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Main started");

        Demo d1 = new Demo();
        Demo d2 = new Demo();
    }
}
```

Output:

```text
Main started
Static block
Constructor
Constructor
```

Why?

The `Demo` class is initialized for the first time when `new Demo()` executes.

Then:

```text
Demo class initialization
        ↓
static field
        ↓
static block
        ↓
new Demo()
        ↓
constructor
```

On the second `new Demo()`:

```text
static block ❌
constructor ✅
```

Because the class is already initialized.

---

# 3.4 Static Block vs Constructor

| Static Block                          | Constructor                        |
| ------------------------------------- | ---------------------------------- |
| Related to class initialization       | Related to object initialization   |
| Usually once per class initialization | Every object creation              |
| No object required                    | Happens with object creation       |
| Can initialize static members         | Initializes instance state         |
| `this` unavailable                    | `this` available                   |

---

# 3.5 Static Block vs Instance Initialization Block

Java also has an instance initialization block:

```java
{
    System.out.println("Instance block");
}
```

Compare:

```java
class Demo {

    static {
        System.out.println("Static block");
    }

    {
        System.out.println("Instance block");
    }

    Demo() {
        System.out.println("Constructor");
    }
}
```

Output when:

```java
new Demo();
```

is executed:

```text
Static block
Instance block
Constructor
```

But the second object:

```java
new Demo();
```

produces:

```text
Instance block
Constructor
```

The static block runs only once.

---

# 3.6 Multiple Static Blocks

Allowed:

```java
class Demo {

    static {
        System.out.println("Block 1");
    }

    static {
        System.out.println("Block 2");
    }
}
```

Execution order:

```text
Block 1
Block 2
```

**Top-to-bottom textual order**.

---

# 3.7 Static Field + Static Block Order

Example:

```java
class Demo {

    static int value = 10;

    static {
        System.out.println(value);
        value = 20;
    }

    static int another = 30;
}
```

Initialization essentially follows textual order:

```text
value = 10
↓
static block
↓
value = 20
↓
another = 30
```

---

# 3.8 Can a static block access instance fields?

Not directly:

```java
class Demo {

    int value = 10;

    static {
        System.out.println(value); // ERROR
    }
}
```

Same fundamental reason:

```text
static context
      ↓
no particular object
      ↓
instance field unavailable directly
```

---

# 3.9 Can a static block have parameters?

No.

This:

```java
static(int value) {
}
```

is invalid syntax.

A static block is not a method.

---

# 3.10 Practical Use Cases

Historically, static blocks were often used for:

* static configuration
* loading JDBC drivers
* complex static initialization
* registering resources
* initializing static collections

Example:

```java
class Config {

    static Map<String, String> settings = new HashMap<>();

    static {
        settings.put("environment", "production");
        settings.put("region", "india");
    }
}
```

Modern applications often prefer cleaner mechanisms depending on the framework/design, but static initialization is still important to understand.

---

# 3.11 Static Block + Main

Consider:

```java
public class Demo {

    static {
        System.out.println("Static block");
    }

    public static void main(String[] args) {
        System.out.println("Main");
    }
}
```

Output:

```text
Static block
Main
```

Why?

Before the JVM invokes:

```java
main()
```

the class containing `main` must be initialized.

So:

```text
Load class
   ↓
Initialize class
   ↓
Execute static initialization
   ↓
Invoke main()
```

---

## 🔑 Must Remember — Static Blocks

* A static block executes at the time of class initialization.
* Object creation is not required.
* A static block generally executes once per class initialization.
* Multiple static blocks are allowed.
* Multiple blocks execute top-to-bottom.
* A static block cannot directly access instance members.
* A static block is not a constructor.
* A constructor executes on every object creation.
* A static block is useful for class-level initialization.

---

# 4. `String[] args` / Command-Line Arguments

Now let's deeply understand:

```java
public static void main(String[] args)
```

---

# 4.1 `public`

```java
public
```

means the method must be accessible to the JVM.

The JVM has to invoke the application entry point.

---

# 4.2 `static`

```java
static
```

means the JVM does not have to create an object in order to call `main()`.

This is important.

Imagine if main were not static:

```java
public void main(...)
```

then the JVM would first have to decide:

```text
Which object should I create?
```

Java's conventional entry point is static:

```java
public static void main(...)
```

so the JVM can invoke the method at the class level.

---

# 4.3 `void`

```java
void
```

means the method does not return any value.

---

# 4.4 `main`

`main` is the specially recognized entry-point method name.

When launching the application, the JVM locates a suitable `main` method.

---

# 4.5 `String[]`

This is the array of command-line arguments.

```java
String[]
```

means:

```text
Array of String
```

Example command:

```text
java Demo John 30
```

Arguments conceptually:

```text
["John", "30"]
```

---

# 4.6 `args`

`args` is just the name of the variable.

This is valid:

```java
public static void main(String[] args)
```

but technically:

```java
public static void main(String[] values)
```

is also valid.

Even:

```java
public static void main(String[] x)
```

is valid.

What matters is:

```java
String[]
```

and the method signature/entry-point convention.

`args` is not a keyword.

---

# 4.7 How does the JVM populate `args`?

Suppose:

```text
java Demo John 30 India
```

The JVM provides the arguments to the application:

```text
args[0] = "John"
args[1] = "30"
args[2] = "India"
```

Notice:

> **All command-line arguments are initially Strings.**

Even:

```text
30
```

is:

```java
"30"
```

not:

```java
30
```

---

# 4.8 `args.length`

```java
public static void main(String[] args) {

    System.out.println(args.length);
}
```

If:

```text
java Demo John 30
```

output:

```text
2
```

If:

```text
java Demo
```

then:

```text
0
```

Important:

> `args` is generally an empty array when no arguments are supplied, not necessarily `null`.

---

# 4.9 Indexing

```java
System.out.println(args[0]);
```

If:

```text
java Demo John 30
```

then:

```text
args[0] → "John"
args[1] → "30"
```

Trying:

```java
args[2]
```

would result in:

```text
ArrayIndexOutOfBoundsException
```

because the array length is 2.

---

# 4.10 Parsing Arguments

Because all values are Strings:

```java
args[1]
```

returns:

```java
"30"
```

If you need an integer:

```java
int age = Integer.parseInt(args[1]);
```

Similarly:

```java
double salary = Double.parseDouble(args[2]);
```

---

# 4.11 Complete Example

```java
public class Demo {

    public static void main(String[] args) {

        if (args.length >= 2) {

            String name = args[0];
            int age = Integer.parseInt(args[1]);

            System.out.println("Name: " + name);
            System.out.println("Age: " + age);
        }
    }
}
```

Run:

```text
java Demo John 30
```

Output:

```text
Name: John
Age: 30
```

---

# 4.12 Java vs Node.js `process.argv`

Java:

```java
public static void main(String[] args)
```

Node.js:

```javascript
process.argv
```

Conceptually:

| Java                             | Node.js                               |
| -------------------------------- | ------------------------------------- |
| `String[] args`                  | `process.argv`                        |
| JVM provides arguments           | Node provides arguments               |
| Arguments are Strings            | Arguments are Strings                 |
| Array                            | Array                                 |
| `args[0]`                        | `process.argv[...]`                   |
| Parse using `Integer.parseInt()` | Parse using `Number()` / `parseInt()` |

One important difference:

Node.js `process.argv` usually includes the Node executable and script path as the initial elements.

For:

```bash
node app.js John 30
```

conceptually:

```javascript
process.argv
```

looks like:

```text
[
  "/path/to/node",
  "/path/to/app.js",
  "John",
  "30"
]
```

Whereas Java:

```text
java Demo John 30
```

gives:

```text
args
↓
["John", "30"]
```

---

## 🔑 Must Remember — `String[] args`

* `args` is just a variable name.
* It represents command-line arguments.
* All arguments are initially `String` values.
* `args.length` gives the number of arguments.
* `args[0]` is the first argument.
* No arguments → empty argument array.
* Numeric conversion has to be done manually.
* In Java, `args` is roughly the equivalent concept of Node.js's `process.argv`.
* `main()` is static so that the JVM does not have to create an object to start the application.

---

# 5. Java Class Initialization & Execution Order

Now let's connect all the concepts together.

Consider:

```java
class Demo {

    static int staticValue = initializeStatic();

    int instanceValue = initializeInstance();

    static {
        System.out.println("Static block");
    }

    {
        System.out.println("Instance initialization block");
    }

    Demo() {
        System.out.println("Constructor");
    }

    static int initializeStatic() {
        System.out.println("Static field initialization");
        return 10;
    }

    int initializeInstance() {
        System.out.println("Instance field initialization");
        return 20;
    }
}

public class Main {

    static {
        System.out.println("Main static block");
    }

    public static void main(String[] args) {

        System.out.println("Main method");

        Demo d1 = new Demo();

        System.out.println("Creating second object");

        Demo d2 = new Demo();
    }
}
```

Let's walk through it.

---

## Step 1 — `Main` class initialization

The JVM has to execute `main()`.

So the `Main` class is initialized.

```java
static {
    System.out.println("Main static block");
}
```

Output:

```text
Main static block
```

---

## Step 2 — `main()` starts

```java
System.out.println("Main method");
```

Output:

```text
Main method
```

---

## Step 3 — `new Demo()`

Now the `Demo` class is being used for the first time.

The JVM will have to initialize the `Demo` class.

First:

```java
static int staticValue = initializeStatic();
```

So:

```text
Static field initialization
```

Output:

```text
Static field initialization
```

Then:

```java
static {
    System.out.println("Static block");
}
```

Output:

```text
Static block
```

Now `Demo` class initialization is complete.

---

## Step 4 — Instance initialization

Now the actual object is being created.

Instance field:

```java
int instanceValue = initializeInstance();
```

executes:

```text
Instance field initialization
```

Then the instance initialization block:

```java
{
    System.out.println("Instance initialization block");
}
```

Then the constructor:

```java
Demo() {
    System.out.println("Constructor");
}
```

So output:

```text
Instance field initialization
Instance initialization block
Constructor
```

---

## Step 5 — Second object

```java
Demo d2 = new Demo();
```

`Demo` is already initialized.

Therefore:

```text
Static field initialization ❌
Static block ❌
```

But a new object is being created:

```text
Instance field initialization
Instance initialization block
Constructor
```

---

# Final Output

Complete output:

```text
Main static block
Main method
Static field initialization
Static block
Instance field initialization
Instance initialization block
Constructor
Creating second object
Instance field initialization
Instance initialization block
Constructor
```

---

# High-Level Execution Model

For interviews, remember it with this mental model:

```text
CLASS INITIALIZATION
        │
        ├── Static field initialization
        │
        ├── Static blocks
        │
        └── Class ready
                 │
                 ▼
              main()
                 │
                 ▼
          new Object()
                 │
                 ├── Instance fields
                 │
                 ├── Instance initialization blocks
                 │
                 └── Constructor
```

One important nuance:

> In the JVM specification, class loading, linking and initialization are technically separate phases. The diagram above is an interview-level simplified model; "class loading" should not be understood as directly equivalent to "static fields execute".

---

# 6. Java vs JavaScript — Important Differences

| Concept                          | Java                                            | JavaScript                                                                               |
| -------------------------------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `static`                         | Class-level field/method                        | Class-level field/method                                                                 |
| Static method                    | `static foo() {}`                               | `static foo() {}`                                                                        |
| Static field                     | `static count = 0;`                             | `static count = 0;`                                                                      |
| `final`                          | Reassignment restriction                        | No direct equivalent                                                                     |
| `const`                          | Closest conceptual comparison to final variable | Prevents variable binding reassignment                                                   |
| Object mutation with final/const | `final User u` can mutate object                | `const user` can mutate object                                                           |
| Static block                     | `static {}`                                     | Static initialization blocks also exist in modern JS classes                             |
| Entry point                      | `main(String[] args)`                           | No fixed equivalent                                                                      |
| CLI args                         | `String[] args`                                 | `process.argv`                                                                           |
| Method overriding                | Instance methods                                | Prototype/class methods                                                                  |
| Static method overriding         | No; hiding                                      | Static methods are inherited through constructor chain but are not instance polymorphism |
| Initialization                   | JVM class initialization semantics              | JS module/class evaluation semantics                                                     |

---

# 7. `final` vs JavaScript `const`

This comparison is a bit subtle.

Java:

```java
final User user = new User();

user = new User(); // ERROR
user.name = "John"; // potentially allowed
```

JavaScript:

```javascript
const user = {};

user = {}; // ERROR

user.name = "John"; // allowed
```

So conceptually:

```text
final reference ≈ const binding
```

But:

> **`final` is not exactly the same as JavaScript `const`.**

In Java, `final` can also be applied to variables, methods, classes and parameters.

JavaScript's `const` is mainly a variable binding declaration.

---

# 8. Frequently Asked Interview Questions

## Q1. What is `static` in Java?

**Expected answer:**

> `static` indicates that a member belongs to the class rather than to individual objects. A static field has class-level state, and a static method can be invoked without creating an instance.

---

## Q2. Why is `main()` static?

**Answer:**

> Because the JVM needs to invoke the application's entry point without first creating an instance of the class.

This is one of the most important reasons.

---

## Q3. Can we access a static member using an object?

Yes.

```java
Demo d = new Demo();

d.staticValue;
```

But preferred:

```java
Demo.staticValue;
```

because the member belongs to the class.

---

## Q4. Can a static method access an instance variable?

Not directly.

```java
class Demo {

    int value;

    static void test() {
        System.out.println(value); // ERROR
    }
}
```

Because no particular object is associated with the static method invocation.

---

## Q5. Why can't static methods use `this`?

Because:

> `this` represents the current object, while a static method is not associated with a particular object.

---

## Q6. Can static methods be overridden?

**No.**

They can be **hidden**.

```java
class Parent {
    static void show() {}
}

class Child extends Parent {
    static void show() {}
}
```

This is method hiding, not overriding.

---

## Q7. What is method hiding?

When a subclass defines a static method with the same signature as a static method in its parent.

The method selected is based on the class/reference type rather than runtime object polymorphism.

---

## Q8. What is `final`?

> `final` prevents reassignment, overriding, or inheritance depending on where it is applied.

* final variable → cannot reassign
* final method → cannot override
* final class → cannot extend

---

## Q9. Difference between `final`, `finally`, and `finalize`?

|              | Meaning                                                                        |
| ------------ | ------------------------------------------------------------------------------ |
| `final`      | Java keyword for restriction                                                   |
| `finally`    | Exception-handling block                                                       |
| `finalize()` | Legacy object finalization mechanism; deprecated and should not be relied upon |

### Interview-safe answer

`finalize()` is particularly important historically, but modern Java code should **not rely on it for resource management**.

Use constructs such as:

```java
try-with-resources
```

for deterministic resource cleanup.

---

## Q10. Is a final object immutable?

**No.**

```java
final User user = new User();
```

means:

```text
user reference cannot be reassigned
```

It does **not** mean:

```text
User object cannot change
```

---

## Q11. What is a static block?

> A static initialization block is executed as part of class initialization and is commonly used for complex initialization of static state.

---

## Q12. When does a static block execute?

When its class is initialized by the JVM.

It generally executes once per class initialization.

---

## Q13. Static block vs constructor?

**Static block:**

```text
class initialization
once
```

**Constructor:**

```text
object initialization
every object
```

---

## Q14. Can we have multiple static blocks?

Yes.

```java
static {
}

static {
}
```

They execute in textual order during class initialization.

---

## Q15. Can we have multiple `main()` methods?

Yes, `main()` can be **overloaded**.

Example:

```java
public static void main(String[] args) {
}

public static void main(int value) {
}
```

But the JVM's application entry point is the recognized `main` signature.

So simply creating:

```java
main(int value)
```

doesn't make it another application entry point.

---

## Q16. Can `main()` be overloaded?

Yes.

But the JVM will look for the valid application entry-point form.

For example:

```java
public static void main(String[] args)
```

is the conventional entry point.

An overloaded:

```java
public static void main(int x)
```

will not replace it.

---

## Q17. What happens if `main()` is not static?

The JVM cannot use it as the standard static application entry point.

For example:

```java
public void main(String[] args)
```

does not satisfy the conventional entry-point requirement.

---

## Q18. Why is `String[] args` an array?

Because an application can receive **zero, one, or many command-line arguments**.

An array naturally represents a variable-length ordered collection.

---

## Q19. Why are command-line arguments Strings?

Because command-line input is textual.

For example:

```text
30
```

arrives as:

```java
"30"
```

If you need an integer:

```java
Integer.parseInt("30");
```

---

## Q20. What happens if no command-line arguments are passed?

Typically:

```java
args.length == 0
```

So:

```java
args[0]
```

would cause an index-out-of-bounds error.

---

# 9. Tricky Interview Code

These types of questions are quite useful in interviews.

### Question

```java
class Parent {

    static int value = 10;

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static int value = 20;

    static void show() {
        System.out.println("Child");
    }
}

public class Main {

    public static void main(String[] args) {

        Parent obj = new Child();

        System.out.println(obj.value);
        obj.show();
    }
}
```

Output:

```text
10
Parent
```

Why?

Because `static` members do not follow runtime polymorphism.

```text
Reference type = Parent
                 ↓
static field/method resolution
                 ↓
Parent
```

This is a classic distinction from instance overriding.

---

# 10. Another Important Trap

```java
class Demo {

    static int value = 10;

    static {
        value = 20;
    }

    public static void main(String[] args) {
        System.out.println(value);
    }
}
```

Output:

```text
20
```

Because:

```text
static field initialization
        ↓
static block
        ↓
main()
```

---

# 11. `static final` vs Instance `final`

Compare:

```java
class Config {

    final int timeout = 5000;

    static final int DEFAULT_TIMEOUT = 5000;
}
```

`timeout`:

```text
per object
```

`DEFAULT_TIMEOUT`:

```text
class-level
```

So:

```java
Config c1 = new Config();
Config c2 = new Config();
```

Conceptually:

```text
c1 → timeout = 5000
c2 → timeout = 5000

Config.DEFAULT_TIMEOUT = 5000
```

For a true application-wide constant, `static final` is usually the more appropriate pattern.

---

# 12. Quick Revision Cheat Sheet

## `static`

```text
static = class-level
```

* One class-level state
* Object not required
* Access: `ClassName.member`
* Static context cannot directly access instance members
* No `this`
* Static methods aren't overridden
* Static fields aren't polymorphic

---

## `final`

```text
final = cannot be reassigned / overridden / extended
```

Depending on usage:

```text
final variable  → cannot reassign
final method    → cannot override
final class     → cannot extend
final parameter → cannot reassign
```

Remember:

```java
final User user
```

does **not** mean an immutable object.

---

## `static final`

```text
static + final = class-level non-reassignable value
```

Commonly:

```java
public static final int MAX_RETRIES = 3;
```

Think:

```text
Class-level constant
```

---

## Static Method

```java
ClassName.method();
```

* No object required
* No `this`
* No direct instance field access
* Cannot be overridden
* Can be hidden

---

## Static Block

```java
static {
    // initialization
}
```

Execution:

```text
Class initialization
      ↓
Static fields
      ↓
Static blocks
      ↓
main / other static usage
```

Generally once per class initialization.

---

## `String[] args`

```java
public static void main(String[] args)
```

Remember:

```text
public → JVM-accessible entry point
static → no object required
void → no return value
main → entry-point method name
String[] → array of strings
args → variable name
```

For:

```text
java Demo John 30
```

you get:

```java
args[0] = "John"
args[1] = "30"
args.length = 2
```

---

# Final Mental Model

If you had to compress these four topics into a single mental model, remember this:

```text
                 JAVA CLASS
                     │
          ┌──────────┴──────────┐
          │                     │
       STATIC                INSTANCE
          │                     │
     Class-level             Object-level
          │                     │
   ┌──────┴──────┐       ┌──────┴──────┐
   │             │       │             │
 static field  method   fields       methods
   │             │
   └──────┬──────┘
          │
    static block
          │
   class initialization
```

And think of `final` as a **restriction** layered on top of this:

```text
final variable → no reassignment
final method   → no overriding
final class    → no inheritance
```

Therefore:

```java
static final int MAX_RETRIES = 3;
```

means:

```text
        CLASS LEVEL
             +
       CANNOT REASSIGN
             ↓
        CONSTANT-LIKE VALUE
```

And connect `main()` to it:

```java
public static void main(String[] args)
       │      │
       │      └── JVM can invoke without object
       │
       └── command-line String array
```

**The most important distinction for interviews:**
`static` is about **class vs object**, whereas `final` is about **reassignment / inheritance / overriding restrictions**. Combining both into `static final` gives you a class-level value that cannot be reassigned.
