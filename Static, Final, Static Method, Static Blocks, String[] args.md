# Java: `static`, `final`, Static Methods, Static Blocks & `String[] args`

---

# 1. `static` and `final` in Java

## 1.1 `static` — Actual Meaning

Java mein `static` ka simplest mental model hai:

> **`static` member class se associated hota hai, kisi particular object/instance se nahi.**

Example:

```java
class User {
    static String applicationName = "MyApp";
    String name;
}
```

Agar:

```java
User u1 = new User();
User u2 = new User();

u1.name = "John";
u2.name = "Mike";
```

To:

```text
u1.name → John
u2.name → Mike
```

kyunki `name` instance field hai.

Lekin:

```java
User.applicationName
```

sirf **ek class-level value** represent karta hai.

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

`u1` aur `u2` ke paas `applicationName` ki separate copy nahi hoti.

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

Ab:

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

object-specific nahi hai.

Har object creation par same static field modify ho raha hai.

### Instance field hota:

```java
class Counter {
    int count = 0;
}
```

To:

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

Whereas static case:

```text
Counter.count = 1
```

and second object ke through increment karoge to:

```text
Counter.count = 2
```

---

# 1.3 Static Member ko kaise access karein?

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

Technically Java object reference se bhi static member access allow karta hai:

```java
Counter c = new Counter();

System.out.println(c.count);
```

Lekin **ye recommended nahi hai**.

Better:

```java
Counter.count
```

because `count` object ka nahi, class ka member hai.

### Interview trap

Question:

> Can static members be accessed using an object?

Answer:

**Yes, Java allows it, but it is discouraged because static members belong to the class, not the object.**

---

# 1.4 Static Field ka lifecycle

Static members class ke initialization ke saath associated hote hain.

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

Static initialization generally **class ke initialization phase mein once** hoti hai.

Ye important hai:

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

Isliye:

```java
e1.company
e2.company
Employee.company
```

teeno same static field ko refer karte hain.

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

But important conceptual point:

> Static members are not polymorphic like instance methods.

Inheritance ke context mein static members ka behavior instance members se different hota hai.

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

Ye **field hiding** ka concept hai, polymorphic overriding nahi.

---

# 1.8 `static` ke important restrictions

Static context ke andar directly instance members access nahi kar sakte.

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

JVM ko pata hi nahi hai ki **kis object's `value`** ki baat ho rahi hai.

Correct:

```java
static void test() {
    Demo d = new Demo();
    System.out.println(d.value);
}
```

---

# 1.9 `final` — Actual Meaning

`final` ka core meaning:

> **Once a final variable is assigned, that variable cannot be assigned another value/reference.**

But `final` ka behavior depend karta hai ki wo kis cheez par apply hua hai:

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

Ye interview ka favourite trap hai.

```java
final User user = new User();
```

Many beginners assume:

> User object immutable hai.

**Wrong.**

`final` yahan **reference ko final** bana raha hai.

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

because `user` reference ko change kar rahe ho.

But:

```java
user.name = "Mike";
```

possible ho sakta hai, assuming `name` mutable hai.

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

> Parent class kisi method ke implementation ko subclasses se change nahi karwana chahta.

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

`String` Java mein final class hai.

---

# 1.14 Final Parameter

```java
void process(final int value) {
    value = 20; // ERROR
}
```

Parameter ko reassign nahi kar sakte.

For reference:

```java
void process(final User user) {
    user.name = "Mike"; // potentially allowed
    user = new User();  // ERROR
}
```

Again same distinction:

```text
final reference ≠ immutable object
```

---

# 1.15 `final` vs Immutability

Ye distinction strongly yaad rakho.

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

Possible if `User` mutable hai.

Immutable object mein:

```java
user.name = "John";
```

allowed hi nahi hoga because object ka state change karne ka mechanism expose nahi kiya gaya.

---

# 1.16 `static final`

Ab dono combine karte hain:

```java
public static final int MAX_RETRIES = 3;
```

Iska meaning:

* `static` → class-level
* `final` → reassignment allowed nahi

So:

```text
MAX_RETRIES
    ↓
one class-level value
    ↓
cannot be reassigned
```

Ye constants ke liye commonly use hota hai.

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

Ye thoda interview-level important distinction hai.

```java
static final int MAX = 10;
```

primitive/String values jo compile time par determine ho sakti hain, **compile-time constants** ho sakti hain.

Example:

```java
static final int MAX = 10;
```

But:

```java
static final int MAX = getMax();
```

ye normal final static field hai, necessarily compile-time constant nahi.

Similarly:

```java
static final Integer MAX = 10;
```

reference type hone ke karan primitive compile-time constant jaisa behavior nahi.

Interview mein broadly yaad rakho:

> `static final` means constant-like class-level value, but every `static final` field is not necessarily a compile-time constant.

---

## 🔑 Must Remember — `static` & `final`

* `static` → class-level association.
* Instance field → object-level state.
* Static field ki single class-level state hoti hai.
* Static members ko preferably `ClassName.member` se access karo.
* Static context directly instance members access nahi kar sakta.
* `final` variable ko reassign nahi kar sakte.
* `final` reference ka matlab object immutable nahi hai.
* `final` method override nahi ho sakta.
* `final` class extend nahi ho sakti.
* `static final` constants ke liye commonly use hota hai.
* `static` + inheritance ≠ normal runtime polymorphism.

---

# 2. Static Methods

## 2.1 Static Method kya hai?

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

Yahan `add()` ko call karne ke liye object ki requirement nahi hai.

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

# 2.3 Static Method kya access kar sakta hai?

Static method directly access kar sakta hai:

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

# 2.4 Why `this` cannot be used?

`this` ka meaning hota hai:

> current object

But static method ko invoke karne ke liye current object required hi nahi.

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

# 2.5 Static Method aur Inheritance

Important interview question:

> Can static methods be overridden?

**No.**

Static methods **override nahi hoti**, they are **hidden**.

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

Ye runtime polymorphism nahi hai.

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

Instance method hota to runtime object determine karta:

```text
reference type → Parent
actual object → Child
```

and overridden instance method → `Child`

But static method mein:

> Method resolution reference/class type ke basis par hota hai, runtime object polymorphism ke basis par nahi.

---

# 2.7 Static Methods kab use karein?

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

But har method ko static banana good design nahi.

Agar operation ko object state ki zarurat hai, instance method more appropriate hai.

---

## 🔑 Must Remember — Static Methods

* Static method class-level hota hai.
* Object create kiye bina call kar sakte ho.
* Static method directly instance fields/methods access nahi kar sakta.
* `this` static method mein available nahi.
* Static methods override nahi hote.
* Static method inheritance mein **hide** ho sakta hai.
* Static method runtime polymorphism participate nahi karta.
* Stateless utility operations ke liye static useful hai.

---

# 3. Static Blocks

Static block Java ka ek particularly important concept hai.

Syntax:

```java
class Demo {

    static {
        System.out.println("Static block");
    }
}
```

---

# 3.1 Static Block kya hai?

Static block ek initialization block hai jo **class initialization ke time execute hota hai**.

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

Static block ka primary purpose:

> **Complex static initialization logic perform karna.**

---

# 3.2 Static Block kab execute hota hai?

Important:

> Static block object create hone par execute nahi hota.

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

Generally ek class initialization ke liye static initialization **once** hoti hai.

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

`Demo` class first time initialized hoti hai jab `new Demo()` execute hota hai.

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

Second `new Demo()` par:

```text
static block ❌
constructor ✅
```

Because class already initialized hai.

---

# 3.4 Static Block vs Constructor

| Static Block                            | Constructor                         |
| --------------------------------------- | ----------------------------------- |
| Class initialization se related         | Object initialization se related    |
| Usually once per class initialization   | Every object creation               |
| Object required nahi                    | Object creation ke saath            |
| Static members initialize kar sakta hai | Instance state initialize karta hai |
| `this` unavailable                      | `this` available                    |

---

# 3.5 Static Block vs Instance Initialization Block

Java mein initialization block bhi hota hai:

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

But second object:

```java
new Demo();
```

produces:

```text
Instance block
Constructor
```

Static block only once.

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

# 3.8 Can static block access instance fields?

Directly no:

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

# 3.9 Can static block have parameters?

No.

This:

```java
static(int value) {
}
```

is invalid syntax.

Static block is not a method.

---

# 3.10 Practical Use Cases

Historically static blocks were often used for:

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

Modern applications often prefer cleaner mechanisms depending on framework/design, but static initialization is still important to understand.

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

Before JVM invokes:

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

* Static block class initialization ke time execute hota hai.
* Object creation required nahi.
* Static block generally once per class initialization execute hota hai.
* Multiple static blocks allowed hain.
* Multiple blocks top-to-bottom execute hote hain.
* Static block instance members directly access nahi kar sakta.
* Static block constructor nahi hai.
* Constructor every object creation par execute hota hai.
* Static block class-level initialization ke liye useful hai.

---

# 4. `String[] args` / Command-Line Arguments

Ab:

```java
public static void main(String[] args)
```

ko deeply understand karte hain.

---

# 4.1 `public`

```java
public
```

means method JVM ke liye accessible hona chahiye.

JVM ko application entry point invoke karna hota hai.

---

# 4.2 `static`

```java
static
```

means JVM ko `main()` call karne ke liye object create nahi karna padta.

This is important.

Imagine agar main static nahi hota:

```java
public void main(...)
```

to JVM ko pehle decide karna padta:

```text
Which object should I create?
```

Java ka conventional entry point static hai:

```java
public static void main(...)
```

so JVM class level par method invoke kar sakti hai.

---

# 4.3 `void`

```java
void
```

means method koi return value nahi deta.

---

# 4.4 `main`

`main` specially recognized entry-point method name hai.

JVM application launch karte waqt suitable `main` method locate karti hai.

---

# 4.5 `String[]`

Ye command-line arguments ka array hai.

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

`args` sirf variable ka naam hai.

Ye valid hai:

```java
public static void main(String[] args)
```

but technically:

```java
public static void main(String[] values)
```

bhi valid hai.

Even:

```java
public static void main(String[] x)
```

valid hai.

Important is:

```java
String[]
```

and method signature/entry-point convention.

`args` keyword nahi hai.

---

# 4.7 JVM `args` ko kaise populate karti hai?

Suppose:

```text
java Demo John 30 India
```

JVM application ko arguments provide karti hai:

```text
args[0] = "John"
args[1] = "30"
args[2] = "India"
```

Notice:

> **All command-line arguments initially Strings hote hain.**

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

> `args` generally empty array hota hai when no arguments are supplied, not necessarily `null`.

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

because array length 2 hai.

---

# 4.10 Parsing Arguments

Because all values Strings hain:

```java
args[1]
```

returns:

```java
"30"
```

If integer chahiye:

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

Node.js `process.argv` usually includes Node executable and script path as initial elements.

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
* All arguments initially `String` values hote hain.
* `args.length` number of arguments deta hai.
* `args[0]` first argument hai.
* No arguments → empty argument array.
* Numeric conversion manually karni hoti hai.
* Java mein `args` roughly Node.js ke `process.argv` ka equivalent concept hai.
* `main()` static hai so JVM ko application start karne ke liye object create nahi karna padta.

---

# 5. Java Class Initialization & Execution Order

Ab sab concepts ko ek saath connect karte hain.

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

JVM ko `main()` execute karna hai.

So `Main` class initialize hoti hai.

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

Ab first time `Demo` class ko use kiya ja raha hai.

JVM ko `Demo` class initialize karni hogi.

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

Ab `Demo` class initialization complete.

---

## Step 4 — Instance initialization

Now actual object create ho raha hai.

Instance field:

```java
int instanceValue = initializeInstance();
```

executes:

```text
Instance field initialization
```

Then instance initialization block:

```java
{
    System.out.println("Instance initialization block");
}
```

Then constructor:

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

`Demo` already initialized hai.

Therefore:

```text
Static field initialization ❌
Static block ❌
```

But new object create ho raha hai:

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

Interview ke liye isko is mental model se yaad rakho:

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

Ek important nuance:

> JVM specification mein class loading, linking aur initialization technically separate phases hain. Upar wala diagram interview-level simplified model hai; "class loading" ko directly "static fields execute" ke equivalent nahi samajhna chahiye.

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

This comparison thoda subtle hai.

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

Java mein `final` variables, methods, classes aur parameters par bhi apply ho sakta hai.

JavaScript `const` mainly variable binding declaration hai.

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

But JVM's application entry point is the recognized `main` signature.

So simply creating:

```java
main(int value)
```

doesn't make it another application entry point.

---

## Q16. Can `main()` be overloaded?

Yes.

But JVM will look for the valid application entry-point form.

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

Array naturally represents a variable-length ordered collection.

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

Ye type ke questions interview mein kaafi useful hain.

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

Because `static` members runtime polymorphism follow nahi karte.

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

does **not** mean immutable object.

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

Agar in chaar topics ko ek single mental model mein compress karna ho, to ye yaad rakho:

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

Aur `final` ko iske upar ek **restriction** samjho:

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

Aur `main()` ko connect karo:

```java
public static void main(String[] args)
       │      │
       │      └── JVM can invoke without object
       │
       └── command-line String array
```

**Interview ke liye sabse important distinction:**
`static` ka relation **class vs object** se hai, jabki `final` ka relation **reassignment / inheritance / overriding restriction** se hai. Dono ko `static final` mein combine karne par ek class-level value milti hai jise reassign nahi kiya ja sakta.
