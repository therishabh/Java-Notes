# Java Notes: `static`, `final`, Static Methods, Static Blocks & `String[] args`

These notes are written assuming you already have strong programming and JavaScript fundamentals. I’ll focus on **how Java actually behaves**, JVM/class-initialization concepts, JavaScript comparisons, and the interview traps that tend to matter for an experienced developer.

---

# 1. `static` and `final` in Java

## 1.1 What does `static` actually mean?

In Java, `static` means that a member belongs to the **class**, rather than to individual instances of that class.

Consider:

```java
class User {
    String name;              // instance field
    static String company;    // static field
}
```

If we create:

```java
User u1 = new User();
User u2 = new User();
```

There are:

* two separate `name` fields — one for `u1`, one for `u2`
* one `company` field associated with the `User` class

Conceptually:

```text
User class
   |
   +---- static company = "ABC"
   |
   +---- u1
   |      name = "John"
   |
   +---- u2
          name = "Mike"
```

Therefore:

```java
u1.company = "Google";
```

and:

```java
System.out.println(u2.company);
```

will print:

```text
Google
```

because both access the **same static field**.

---

## 1.2 Static field vs instance field

```java
class Counter {

    int instanceCount = 0;
    static int staticCount = 0;

    void increment() {
        instanceCount++;
        staticCount++;
    }
}
```

Now:

```java
Counter c1 = new Counter();
Counter c2 = new Counter();

c1.increment();
c2.increment();
```

Result:

```text
c1.instanceCount = 1
c2.instanceCount = 1

Counter.staticCount = 2
```

Why?

`instanceCount` belongs to each object:

```text
c1 → instanceCount = 1
c2 → instanceCount = 1
```

`staticCount` is shared:

```text
Counter
  └── staticCount = 2
```

### Interview-level takeaway

> `static` changes the ownership model from **object-level** to **class-level**.

It does **not** simply mean "global variable."

---

# 1.3 How do you access static members?

Preferred syntax:

```java
class Config {
    static String environment = "PROD";
}

System.out.println(Config.environment);
```

Use the class name:

```java
Config.environment
```

rather than:

```java
Config config = new Config();
config.environment;
```

Java technically permits accessing a static member through an object reference:

```java
Config config = new Config();

System.out.println(config.environment);
```

But this is misleading because the field doesn't belong to `config`.

It belongs to `Config`.

### Interview trap

**Q: Can static members be accessed through objects?**

**Answer:** Yes, Java allows it, but it should generally be accessed using the class name because static members belong to the class.

---

# 1.4 Static variable lifecycle

A static field is associated with the class and exists as long as the relevant class remains initialized/usable by its class loader.

This is different from an instance field, whose lifetime is tied to an object.

Conceptually:

```text
Class loaded/initialized
        ↓
Static fields initialized
        ↓
Static members available
        ↓
Objects may be created
```

One important JVM nuance:

> Static fields are associated with the `Class`/class-loader context, not with individual objects.

This becomes particularly important in application servers where the same class may be loaded by different class loaders.

---

# 1.5 Static with inheritance

Consider:

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

But conceptually the static field is still declared by `Parent`.

Static members are **not polymorphic in the same way instance methods are**.

That distinction becomes important with static methods.

---

# 1.6 Important restrictions of `static`

A static context does not have a specific object instance.

Therefore:

```java
class Demo {

    int value = 10;

    static void test() {
        System.out.println(value); // ERROR
    }
}
```

Why?

Because Java would have no idea which object's `value` you mean.

There could be:

```java
Demo d1;
Demo d2;
Demo d3;
```

A static method belongs to the class, not any particular object.

You must explicitly provide an object:

```java
static void test(Demo demo) {
    System.out.println(demo.value);
}
```

---

# 1.7 What is `final`?

`final` means that something cannot be changed in the particular way that `final` restricts.

But **what cannot change depends on what `final` is applied to**.

| Applied to | Meaning                                  |
| ---------- | ---------------------------------------- |
| Variable   | Cannot be assigned again                 |
| Reference  | Reference cannot point to another object |
| Method     | Cannot be overridden                     |
| Class      | Cannot be extended                       |
| Parameter  | Parameter cannot be reassigned           |

This distinction is extremely important.

---

# 1.8 Final primitive

```java
final int age = 30;
```

This is illegal:

```java
age = 40;
```

because the variable cannot be reassigned.

For primitives, this effectively means the stored value cannot be changed through reassignment.

---

# 1.9 Final reference

This is where many developers coming from JavaScript get confused.

```java
final User user = new User();
```

The reference `user` cannot be reassigned:

```java
user = new User(); // ERROR
```

But the object itself may still be mutable:

```java
user.name = "John";
user.age = 35;
```

This is perfectly legal if those fields are mutable.

Conceptually:

```text
user
 |
 | final reference
 ↓
+----------------+
| User object    |
| name = "John"  |
| age = 35       |
+----------------+
```

`final` protects the **reference**, not the object.

### Very important

```java
final User user = new User();
```

does **NOT** mean:

> "This User object is immutable."

It means:

> "The variable `user` cannot be made to reference another object."

---

# 1.10 `final` vs immutability

These are different concepts.

### Final reference

```java
final User user = new User();

user.setName("John");
```

Possible.

### Immutable object

An immutable object's state cannot be changed after construction.

For example, `String` is immutable.

```java
String name = "John";
```

Operations that appear to modify a String actually produce another String.

```java
name = name.toUpperCase();
```

The reference was reassigned to another String.

So:

> `final` is a language-level restriction on reassignment/extension/overriding.
> Immutability is an object-design property.

---

# 1.11 Final method

```java
class Parent {

    final void calculate() {
        System.out.println("Calculation");
    }
}
```

A subclass cannot override it:

```java
class Child extends Parent {

    void calculate() { // ERROR
    }
}
```

Why use it?

When a superclass wants to guarantee that a particular implementation cannot be replaced by subclasses.

---

# 1.12 Final class

```java
final class SecurityManager {
}
```

This is illegal:

```java
class CustomSecurityManager extends SecurityManager {
}
```

A final class cannot be subclassed.

Common examples include:

```java
String
```

`String` is final.

This prevents subclasses from changing its behavior and is part of its design for immutability and security.

---

# 1.13 Final parameter

```java
void process(final int count) {
    count = 10; // ERROR
}
```

The method cannot reassign `count`.

It doesn't make an object passed as a parameter immutable.

---

# 1.14 `static final`

This is one of the most common combinations in Java.

```java
public static final int MAX_RETRIES = 3;
```

Break it down:

* `static` → one class-level member
* `final` → cannot be reassigned
* `int` → primitive type

Therefore:

```java
MAX_RETRIES
```

is effectively a class-level constant.

Usage:

```java
System.out.println(Config.MAX_RETRIES);
```

---

## Constant naming convention

Java convention:

```java
static final int MAX_RETRIES = 3;
static final String DEFAULT_REGION = "IN";
static final double TAX_RATE = 0.18;
```

Use:

```text
UPPER_SNAKE_CASE
```

---

# 1.15 Compile-time constants

An important interview distinction:

```java
static final int MAX = 100;
```

can be a **compile-time constant** when initialized with a constant expression.

For example:

```java
static final int MAX = 10 * 10;
```

But:

```java
static final int MAX = getMax();
```

is not a compile-time constant because its value requires runtime evaluation.

This distinction becomes relevant in:

* `switch`
* constant folding
* class initialization
* binary compatibility
* inlining

---

# 1.16 `static final` does not mean immutable object

Consider:

```java
static final User USER = new User();
```

The reference cannot be changed:

```java
USER = new User(); // ERROR
```

But:

```java
USER.setName("John");
```

could still be legal.

So:

```text
static final
     ↓
same class-level reference
     +
cannot reassign reference
```

It does **not automatically mean immutable object**.

---

# 🔑 Must Remember — `static` & `final`

1. `static` means **class-level**, not object-level.
2. Static fields are shared across instances of the same class-loading context.
3. Prefer `ClassName.staticMember`.
4. Static methods don't have an implicit object instance.
5. `final` variable cannot be reassigned.
6. `final` reference does not make the referenced object immutable.
7. `final` method cannot be overridden.
8. `final` class cannot be extended.
9. `static final` is commonly used for constants.
10. `static final` does not automatically imply deep immutability.

---

# 2. Static Methods

## 2.1 What is a static method?

A static method belongs to the class rather than an instance.

```java
class MathUtils {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call it:

```java
int result = MathUtils.add(10, 20);
```

You don't need:

```java
MathUtils mathUtils = new MathUtils();
```

because the method doesn't require an object instance.

---

# 2.2 Static method vs instance method

```java
class UserService {

    static String getApplicationName() {
        return "MyApp";
    }

    String getUserName() {
        return "John";
    }
}
```

Static:

```java
UserService.getApplicationName();
```

Instance:

```java
UserService service = new UserService();

service.getUserName();
```

The conceptual difference:

```text
static method
     ↓
Class-level behavior

instance method
     ↓
Object-level behavior
```

---

# 2.3 Why can't static methods access instance fields directly?

Example:

```java
class User {

    String name;

    static void printName() {
        System.out.println(name); // ERROR
    }
}
```

Because `name` belongs to an object.

There might be:

```java
User u1; // name = John
User u2; // name = Mike
```

Which name should the static method use?

There is no implicit object.

---

# 2.4 Static method can access static members

```java
class Counter {

    static int count = 10;

    static void printCount() {
        System.out.println(count);
    }
}
```

This works because both are class-level.

---

# 2.5 Static method can access instance members through an object

This is legal:

```java
class User {

    String name = "John";

    static void printUser(User user) {
        System.out.println(user.name);
    }
}
```

The difference is that the object is explicit:

```java
User.printUser(user);
```

---

# 2.6 Why can't `this` be used in static methods?

`this` represents the **current object instance**.

A static method has no current object.

Therefore:

```java
static void test() {
    System.out.println(this); // ERROR
}
```

This is one of the cleanest ways to understand the distinction:

```text
Instance method
    ↓
has implicit object context
    ↓
this available

Static method
    ↓
no implicit object context
    ↓
this unavailable
```

---

# 2.7 Static method inheritance

Static methods can be inherited, depending on accessibility and inheritance rules, but they do not participate in runtime overriding like instance methods.

Consider:

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
}
```

You can write:

```java
Child.show();
```

But this doesn't mean `Child` has polymorphically overridden the method.

---

# 2.8 Can static methods be overridden?

**No.**

Static methods are not overridden.

They can be **hidden**.

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
```

prints:

```text
Parent
```

and:

```java
Child.show();
```

prints:

```text
Child
```

This is **method hiding**.

---

# 2.9 Method hiding vs method overriding

### Instance method

```java
Parent p = new Child();

p.show();
```

If `show()` is overridden, runtime dispatch selects:

```text
Child.show()
```

### Static method

```java
Parent p = new Child();

p.show();
```

If `show()` is static, selection is based on the **reference type**, not runtime object type.

Conceptually:

```text
Static → compile-time/reference-type based
Instance → runtime polymorphism
```

This is a very common interview question.

---

# 2.10 When should you use static methods?

Good candidates are methods that:

* don't depend on object state
* represent utility behavior
* don't require polymorphism
* are conceptually associated with the class

Examples:

```java
Math.max()
Math.min()
Integer.parseInt()
Collections.sort()
```

A utility class might contain:

```java
class StringUtils {

    static boolean isBlank(String value) {
        return value == null || value.isEmpty();
    }
}
```

---

# 2.11 When should you NOT use static?

Avoid static when behavior should depend on:

* object state
* dependency injection
* polymorphism
* runtime implementation selection

For example, this is often better as an instance/service abstraction:

```java
interface PaymentService {
    void pay();
}
```

rather than putting everything into:

```java
PaymentUtils.pay();
```

Static-heavy designs can make testing, dependency replacement, and polymorphism harder.

---

# 🔑 Must Remember — Static Methods

1. Static methods belong to the class.
2. Call them using `ClassName.method()`.
3. They don't have an implicit `this`.
4. They cannot directly access instance fields/methods.
5. They can directly access static members.
6. Static methods are not overridden.
7. Static methods can be hidden.
8. Static method dispatch is not runtime polymorphism.
9. Use static when behavior doesn't require object state.

---

# 3. Static Blocks

Static blocks are one of the more Java-specific concepts.

## 3.1 What is a static block?

A static block is a block of code executed as part of **class initialization**.

Syntax:

```java
class Demo {

    static {
        System.out.println("Static block");
    }
}
```

It has no method name and no parameters.

---

# 3.2 When does a static block execute?

A static block executes when the class is **initialized by the JVM**, typically before the first active use such as invoking a static method or creating an instance.

For:

```java
class Demo {

    static {
        System.out.println("Static block");
    }

    public static void main(String[] args) {
        System.out.println("main");
    }
}
```

Output:

```text
Static block
main
```

Why?

Before the JVM invokes `main`, the class must be initialized.

During class initialization, the static initialization logic executes.

---

# 3.3 Static field + static block

Consider:

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

If:

```java
Demo demo = new Demo();
```

the relevant sequence is:

```text
class initialization
    ↓
static field initialization
    ↓
static block
    ↓
instance creation
    ↓
constructor
```

So:

```text
Static block
Constructor
```

---

# 3.4 Static field initialization and static block ordering

Order matters.

```java
class Demo {

    static int value = 10;

    static {
        value = 20;
    }

    static int anotherValue = 30;
}
```

The JVM executes static initialization logic in textual order.

Conceptually:

```text
value = 10
    ↓
static block
value = 20
    ↓
anotherValue = 30
```

Therefore:

```java
System.out.println(value);
```

gives:

```text
20
```

---

# 3.5 Multiple static blocks

You can have multiple static blocks:

```java
class Demo {

    static {
        System.out.println("Block 1");
    }

    static {
        System.out.println("Block 2");
    }

    static {
        System.out.println("Block 3");
    }
}
```

They execute in source order:

```text
Block 1
Block 2
Block 3
```

during class initialization.

---

# 3.6 Static block vs constructor

| Static Block                           | Constructor                 |
| -------------------------------------- | --------------------------- |
| Class initialization                   | Object initialization       |
| Runs when class is initialized         | Runs when object is created |
| Normally once per class initialization | Once per object creation    |
| Can initialize static state            | Initializes instance state  |
| No object required                     | Requires object creation    |
| Cannot use `this`                      | Can use `this`              |

Example:

```java
class Demo {

    static {
        System.out.println("Static");
    }

    Demo() {
        System.out.println("Constructor");
    }
}
```

Creating:

```java
new Demo();
new Demo();
```

typically produces:

```text
Static
Constructor
Constructor
```

The static initialization occurs once for that class initialization, while the constructor executes for each object.

---

# 3.7 Static block vs instance initialization block

Java also has instance initialization blocks:

```java
class Demo {

    {
        System.out.println("Instance block");
    }

    Demo() {
        System.out.println("Constructor");
    }
}
```

Execution:

```text
Instance block
Constructor
```

For every object.

Compare:

```java
static {
    // class-level initialization
}
```

with:

```java
{
    // instance-level initialization
}
```

---

# 3.8 Can static blocks access instance variables?

Not directly.

```java
class Demo {

    int value = 10;

    static {
        System.out.println(value); // ERROR
    }
}
```

Same reason as static methods.

There is no current object.

---

# 3.9 Can static blocks have parameters?

No.

This isn't valid:

```java
static(int x) {
}
```

A static block isn't a method.

It is part of the class initialization mechanism.

---

# 3.10 Practical uses of static blocks

Static blocks can be useful for:

* complex static initialization
* initializing static resources
* registration logic
* loading certain configuration
* initialization that requires multiple statements
* legacy initialization patterns

Example:

```java
class Config {

    static Map<String, String> settings;

    static {
        settings = new HashMap<>();
        settings.put("env", "prod");
        settings.put("region", "india");
    }
}
```

That said, modern Java code often prefers clearer initialization approaches where appropriate.

---

# 🔑 Must Remember — Static Blocks

1. Static blocks run during class initialization.
2. They execute before `main()` when the class containing `main()` is initialized.
3. Static field initializers and static blocks execute in textual order.
4. Multiple static blocks execute in source order.
5. A static block doesn't require an object.
6. Static blocks cannot directly access instance members.
7. Constructors run per object; static initialization runs per class initialization.
8. Static blocks are mainly for class-level initialization.

---

# 4. `String[] args` / Command-Line Arguments

Now let's break down:

```java
public static void main(String[] args)
```

---

# 4.1 `public`

```java
public
```

The method is accessible to the JVM launcher.

The JVM needs an accessible entry point to invoke your application.

---

# 4.2 `static`

```java
static
```

The JVM can invoke `main()` without creating an object of your application class.

Imagine if `main()` were an instance method:

```java
Demo demo = new Demo();
demo.main();
```

The JVM would first need to know how and when to construct the object.

Java's application entry point therefore uses a static method.

---

# 4.3 `void`

```java
void
```

`main()` doesn't return a value to the caller through its method signature.

---

# 4.4 `main`

```java
main
```

This is the conventional entry-point method recognized by the Java launcher.

It is not a magical keyword.

It is a specially recognized method name/signature for launching a Java application.

---

# 4.5 `String[]`

```java
String[]
```

This means:

> an array whose elements are `String` objects.

For example:

```java
String[] names = {
    "John",
    "Mike",
    "Sarah"
};
```

---

# 4.6 `args`

```java
args
```

is simply the parameter variable name.

You could technically write:

```java
public static void main(String[] arguments)
```

and:

```java
public static void main(String[] values)
```

The conventional name is:

```java
args
```

---

# 4.7 Where does `args` come from?

When you launch:

```bash
java Demo John 30
```

the command-line launcher passes those arguments to `main()`.

Conceptually:

```text
java Demo John 30
          │    │
          │    └── args[1]
          └─────── args[0]
```

So:

```java
public static void main(String[] args) {

    System.out.println(args[0]);
    System.out.println(args[1]);
}
```

outputs:

```text
John
30
```

---

# 4.8 Everything is initially a String

This is important.

If you execute:

```bash
java Demo John 30
```

then:

```java
args[0]
```

is:

```java
"John"
```

and:

```java
args[1]
```

is:

```java
"30"
```

It is **not** an integer.

Therefore:

```java
int age = Integer.parseInt(args[1]);
```

converts:

```text
"30"
```

into:

```text
30
```

Similarly:

```java
double price = Double.parseDouble(args[1]);
```

---

# 4.9 What happens with no arguments?

If you run:

```bash
java Demo
```

then:

```java
args.length
```

is:

```text
0
```

But:

```java
args[0]
```

will cause:

```text
ArrayIndexOutOfBoundsException
```

because there is no element at index `0`.

So this is safer:

```java
if (args.length > 0) {
    System.out.println(args[0]);
}
```

---

# 4.10 Example

```java
public class Demo {

    public static void main(String[] args) {

        System.out.println("Number of arguments: " + args.length);

        for (String arg : args) {
            System.out.println(arg);
        }
    }
}
```

Run:

```bash
java Demo John 30 India
```

Output:

```text
Number of arguments: 3
John
30
India
```

---

# 4.11 Parsing arguments

```java
public static void main(String[] args) {

    String name = args[0];
    int age = Integer.parseInt(args[1]);

    System.out.println(name);
    System.out.println(age);
}
```

Run:

```bash
java Demo Rishabh 35
```

Conceptually:

```text
args
 ├── [0] → "Rishabh"
 └── [1] → "35"
```

Then:

```java
Integer.parseInt("35")
```

produces:

```text
35
```

---

# 4.12 Java vs Node.js `process.argv`

Since you already know Node.js, this comparison is useful.

| Concept           | Java                 | Node.js                   |
| ----------------- | -------------------- | ------------------------- |
| Command-line args | `String[] args`      | `process.argv`            |
| Type              | `String[]`           | JavaScript Array          |
| Access            | `args[0]`            | `process.argv[n]`         |
| Input             | Strings              | Strings                   |
| Count             | `args.length`        | `process.argv.length`     |
| Parsing number    | `Integer.parseInt()` | `Number()` / `parseInt()` |
| Runtime           | JVM                  | Node.js runtime           |

There is one important indexing difference.

Java:

```bash
java Demo John 30
```

gives:

```text
args[0] = "John"
args[1] = "30"
```

Node:

```bash
node app.js John 30
```

typically gives:

```text
process.argv[0] = path to Node
process.argv[1] = path to script
process.argv[2] = "John"
process.argv[3] = "30"
```

So Java's `args` is closer to:

```javascript
process.argv.slice(2)
```

than the entire `process.argv`.

---

# 🔑 Must Remember — `String[] args`

1. `args` is a `String[]`.
2. It contains command-line arguments supplied to the Java launcher.
3. Arguments are strings initially.
4. `args.length` tells you how many arguments were provided.
5. Access using zero-based indexing.
6. `args[0]` is the first user-supplied argument.
7. Numeric arguments require explicit parsing.
8. `args[0]` without checking `args.length` can throw an exception.
9. It is conceptually similar to Node's `process.argv.slice(2)`.

---

# 5. Java Class Initialization & Execution Order

This is the section I'd pay particular attention to for interviews.

Consider:

```java
class Demo {

    static int staticValue = initializeStatic();

    static {
        System.out.println("Static block");
    }

    int instanceValue = initializeInstance();

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

    public static void main(String[] args) {

        System.out.println("main");

        Demo demo = new Demo();
    }
}
```

Output:

```text
Static field initialization
Static block
main
Instance field initialization
Instance initialization block
Constructor
```

Let's understand why.

---

## Step 1 — Class initialization

Before `main()` executes, the JVM initializes the class.

Static initialization occurs.

```java
static int staticValue = initializeStatic();
```

So:

```text
Static field initialization
```

is printed.

---

## Step 2 — Static block

Next:

```java
static {
    System.out.println("Static block");
}
```

prints:

```text
Static block
```

Remember:

> Static field initializers and static blocks execute in textual order.

---

## Step 3 — `main()`

Now the JVM invokes:

```java
main()
```

So:

```text
main
```

prints.

---

## Step 4 — Object creation

Inside `main()`:

```java
Demo demo = new Demo();
```

Now instance initialization starts.

---

## Step 5 — Instance field initialization

```java
int instanceValue = initializeInstance();
```

prints:

```text
Instance field initialization
```

---

## Step 6 — Instance initialization block

Then:

```java
{
    System.out.println("Instance initialization block");
}
```

prints:

```text
Instance initialization block
```

---

## Step 7 — Constructor

Finally:

```java
Demo() {
    System.out.println("Constructor");
}
```

prints:

```text
Constructor
```

---

# Simplified lifecycle

For a normal application class:

```text
Class initialization
        ↓
Static field initializers
        ↓
Static blocks
        ↓
main()
        ↓
new Object()
        ↓
Instance field initializers
        ↓
Instance initialization blocks
        ↓
Constructor
```

One nuance:

**Class loading and class initialization are not identical concepts.**

The JVM may load a class before it actually initializes it. Static initialization happens during the class initialization phase, when required by JVM rules.

For interview purposes, remember:

> Loading makes the class available to the JVM; initialization executes its static initialization logic.

---

# 6. JavaScript vs Java — Important Comparison

| Concept              | Java                                                        | JavaScript                          | Important Difference                                                                          |
| -------------------- | ----------------------------------------------------------- | ----------------------------------- | --------------------------------------------------------------------------------------------- |
| `static` field       | Class-level field                                           | Class-level field                   | Similar syntax/intent, but Java has Java's class/type system and JVM initialization semantics |
| `static` method      | Class-level method                                          | Class-level method                  | Both don't require an instance                                                                |
| `final`              | Prevents reassignment/override/extension depending on usage | No direct equivalent                | `const` only prevents variable reassignment                                                   |
| `final` reference    | Reference can't point elsewhere                             | `const` reference behaves similarly | Object itself can still mutate in both cases                                                  |
| Immutable object     | Separate design property                                    | Separate design property            | `final` does not imply immutability                                                           |
| Static block         | Java class initialization feature                           | No direct equivalent                | JS has static initialization blocks, but semantics are based on JS class evaluation           |
| `main()`             | Conventional JVM application entry point                    | No equivalent                       | Node starts from the module/script being executed                                             |
| CLI arguments        | `String[] args`                                             | `process.argv`                      | Node includes executable/script paths                                                         |
| Class initialization | JVM-managed                                                 | JS engine/module/class evaluation   | Different runtime models                                                                      |
| Method overriding    | Runtime polymorphism for instance methods                   | Prototype-based dispatch            | Java static methods don't participate in overriding                                           |
| Type of CLI args     | `String[]`                                                  | Array of strings                    | Both require parsing for numbers                                                              |

---

# 7. Java `static` vs JavaScript `static`

Java:

```java
class User {

    static String role = "ADMIN";

    static void printRole() {
        System.out.println(role);
    }
}
```

Usage:

```java
User.printRole();
```

JavaScript:

```javascript
class User {
    static role = "ADMIN";

    static printRole() {
        console.log(User.role);
    }
}
```

Usage:

```javascript
User.printRole();
```

The basic usage looks very similar.

But Java's behavior is tied to:

* Java class definitions
* JVM class initialization
* static typing
* class loaders
* JVM method/field resolution

JavaScript's behavior is defined by ECMAScript's class semantics and JavaScript's object/prototype model.

---

# 8. Java `final` vs JavaScript `const`

This is a particularly important distinction.

### Java

```java
final User user = new User();

user.setName("John");

user = new User(); // ERROR
```

### JavaScript

```javascript
const user = {
    name: "John"
};

user.name = "Mike"; // valid

user = {}; // ERROR
```

So at the **reference binding** level, they're similar.

But Java `final` is broader:

```java
final class User {}
```

prevents inheritance.

And:

```java
final void calculate() {}
```

prevents overriding.

JavaScript `const` doesn't have those meanings.

---

# 9. Tricky Interview Questions

## Q1. Why is `main()` static?

**Expected answer:**

> `main()` is static so the JVM can invoke the application's entry point without first creating an instance of the class.

---

## Q2. Can a static method access an instance variable?

**Answer:**

Not directly.

```java
class Demo {

    int value;

    static void test() {
        System.out.println(value); // ERROR
    }
}
```

It needs an explicit object:

```java
static void test(Demo demo) {
    System.out.println(demo.value);
}
```

---

## Q3. Why can't static methods use `this`?

**Answer:**

`this` represents the current object instance.

A static method belongs to the class and doesn't have an implicit current object.

---

## Q4. Can static methods be overridden?

**Expected answer:**

> No. Static methods are not overridden; they are hidden when a subclass declares a static method with the same signature.

---

## Q5. What is method hiding?

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

The subclass method hides the superclass static method.

This is different from runtime overriding.

---

## Q6. Can we access a static member using an object?

Yes:

```java
User user = new User();

user.someStaticMethod();
```

But this is discouraged because it makes the code look like the member belongs to the object.

Prefer:

```java
User.someStaticMethod();
```

---

## Q7. What is `final`?

A strong interview answer:

> `final` restricts modification depending on where it is applied. A final variable cannot be reassigned, a final method cannot be overridden, and a final class cannot be extended.

---

## Q8. Is a final object immutable?

**No.**

```java
final User user = new User();

user.name = "John";
```

The object can still change if its class is mutable.

`final` applies to the reference, not automatically to the object's internal state.

---

# 10. `final`, `finally`, and `finalize`

Very common interview question.

| Term         | Purpose                                                                     |
| ------------ | --------------------------------------------------------------------------- |
| `final`      | Restricts reassignment, overriding, or inheritance                          |
| `finally`    | Exception-handling block that normally executes after `try`/`catch`         |
| `finalize()` | Old object-finalization mechanism; deprecated and should not be relied upon |

Example:

```java
final int x = 10;
```

versus:

```java
try {
    // code
} finally {
    // cleanup
}
```

`finalize()` is a completely different concept and has been deprecated for removal in modern Java.

For modern code, don't use it for resource management.

---

# 11. Can we have multiple static blocks?

Yes.

```java
class Demo {

    static {
        System.out.println("1");
    }

    static {
        System.out.println("2");
    }

    static {
        System.out.println("3");
    }
}
```

Output:

```text
1
2
3
```

They execute in source order during class initialization.

---

# 12. Can we have multiple `main()` methods?

Yes, through **overloading**.

For example:

```java
public static void main(String[] args) {
    System.out.println("Entry point");
}

public static void main(int value) {
    System.out.println(value);
}
```

But the JVM launcher looks for the recognized entry-point signature.

It will invoke:

```java
main(String[] args)
```

not:

```java
main(int value)
```

### Important

You can overload `main()`.

You cannot have two methods with exactly the same signature.

---

# 13. What happens if `main()` is not static?

If the expected application entry point is:

```java
public void main(String[] args)
```

rather than:

```java
public static void main(String[] args)
```

the JVM launcher does not have the required static entry point and the application won't launch normally through the standard Java launcher.

The key reason remains:

> JVM needs to invoke the entry point without creating an instance first.

---

# 14. Execution Order Interview Example

Consider:

```java
class Test {

    static int a = print("Static field");

    static {
        print("Static block");
    }

    int b = print("Instance field");

    {
        print("Instance block");
    }

    Test() {
        print("Constructor");
    }

    static int print(String message) {
        System.out.println(message);
        return 1;
    }

    public static void main(String[] args) {

        print("main");

        new Test();
        new Test();
    }
}
```

Expected output:

```text
Static field
Static block
main
Instance field
Instance block
Constructor
Instance field
Instance block
Constructor
```

Why?

### Before `main()`

```text
Static field
Static block
```

Class initialization happens.

### `main()`

```text
main
```

### First `new Test()`

```text
Instance field
Instance block
Constructor
```

### Second `new Test()`

```text
Instance field
Instance block
Constructor
```

Static initialization does **not** repeat merely because another object is created.

---

# 15. High-Value Interview Traps

### Trap 1

```java
final User user = new User();
```

Doesn't mean immutable.

---

### Trap 2

```java
Parent p = new Child();
p.staticMethod();
```

Don't apply normal runtime polymorphism rules to static methods.

Static method selection is based on the reference/type context rather than dynamic dispatch.

---

### Trap 3

```java
static void method() {
    this.value++;
}
```

Impossible.

No `this` in static context.

---

### Trap 4

```java
static int x = 10;

static {
    x = 20;
}
```

Final value:

```text
20
```

because static initialization follows source order.

---

### Trap 5

```java
System.out.println(args[0]);
```

This is unsafe if no command-line arguments were supplied.

---

### Trap 6

```java
static final User user = new User();
```

The reference is fixed, but the object may still be mutable.

---

# 16. Quick Revision Cheat Sheet

| Topic                    | Core Rule                                              |
| ------------------------ | ------------------------------------------------------ |
| `static` field           | Belongs to class                                       |
| Instance field           | Belongs to object                                      |
| Static field             | Shared between instances in same class-loading context |
| Static method            | Class-level behavior                                   |
| `this` in static method  | Not allowed                                            |
| Static → instance field  | Not directly allowed                                   |
| Static method overriding | No                                                     |
| Static method hiding     | Yes                                                    |
| `final` variable         | Cannot be reassigned                                   |
| `final` reference        | Cannot reference another object                        |
| `final` object           | Does not automatically mean immutable                  |
| `final` method           | Cannot be overridden                                   |
| `final` class            | Cannot be extended                                     |
| `static final`           | Common pattern for constants                           |
| Static block             | Runs during class initialization                       |
| Multiple static blocks   | Execute in source order                                |
| Constructor              | Runs for every object                                  |
| Static initialization    | Normally once per class initialization                 |
| `main()`                 | Standard Java application entry point                  |
| `main()` static          | JVM can invoke without object                          |
| `String[] args`          | Command-line arguments                                 |
| `args[0]`                | First user-supplied CLI argument                       |
| CLI argument type        | String                                                 |
| Number argument          | Explicit parsing required                              |
| No CLI args              | `args.length == 0`                                     |
| Node equivalent          | Roughly `process.argv.slice(2)`                        |

---

# Final Mental Model

If you remember only one conceptual model from these topics, make it this:

```text
                    JAVA CLASS
                       │
          ┌────────────┴────────────┐
          │                         │
       STATIC                    INSTANCE
          │                         │
          │                         │
   belongs to class          belongs to object
          │                         │
   static fields              instance fields
   static methods             instance methods
   static blocks              instance blocks
          │                         │
          └────────────┬────────────┘
                       │
                  object creation
                       │
                       ▼
                  constructor
```

And:

```text
final
 │
 ├── variable      → cannot reassign
 ├── reference     → cannot point elsewhere
 ├── method        → cannot override
 └── class         → cannot extend
```

And the application startup model:

```text
JVM launches application
        ↓
Class initialization
        ↓
Static field initializers
        ↓
Static blocks
        ↓
main(String[] args)
        ↓
new Object()
        ↓
Instance field initialization
        ↓
Instance initialization blocks
        ↓
Constructor
```

The three distinctions I'd especially lock in for interviews are:

> **`static` ≠ object-level state**

> **`final` reference ≠ immutable object**

> **static method hiding ≠ instance method overriding**
