# Java Notes — String, OOP, Classes/Objects, Constructors & Abstract Classes

---

# 1. String

## 1.1 Concept

Java mein `String` ek **class** hai, primitive data type nahi.

```java
String name = "Rishabh";
```

JavaScript ke comparison mein ye important difference hai:

```javascript
const name = "Rishabh";
```

JavaScript mein string primitive hai, whereas Java mein:

```java
String
```

`java.lang.String` class hai.

Lekin Java String ko language level par special treatment milta hai, especially **String literals** aur **String Pool** ke through.

---

# 1.2 String Creation

### String literal

```java
String name = "Rishabh";
```

Ye most common approach hai.

### Using `new`

```java
String name = new String("Rishabh");
```

Technically valid hai, lekin normally unnecessary hai.

Preferred:

```java
String name = "Rishabh";
```

### Why?

String literals JVM ke **String Pool** mein intern ho sakte hain, jisse identical strings reuse ki ja sakti hain.

---

# 1.3 String Pool

Consider:

```java
String a = "Java";
String b = "Java";
```

Conceptually:

```text
String Pool

       "Java"
        ↑  ↑
        │  │
        a  b
```

`a` aur `b` same pooled String object ko refer kar sakte hain.

But:

```java
String c = new String("Java");
```

`c` explicitly new String object create karta hai.

Conceptually:

```text
String Pool
   "Java"
      ↑
      a

Heap
   "Java"
      ↑
      c
```

---

# 1.4 `==` vs `.equals()`

**Extremely important interview topic.**

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Typically:

```text
true
```

because both same pooled object ko reference karte hain.

But:

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Result:

```text
false
```

because references different objects ko point karte hain.

But:

```java
System.out.println(a.equals(b));
```

Result:

```text
true
```

because `.equals()` String **content** compare karta hai.

### Mental model

```text
==

"Are these references pointing to the same object?"

.equals()

"Do these objects have equal content?"
```

---

# 1.5 JavaScript Comparison

JavaScript:

```javascript
"Java" === "Java"
```

generally content equality deta hai for primitive strings.

Java:

```java
a == b
```

reference identity compare karta hai.

Therefore:

> **Java mein String comparison ke liye generally `.equals()` use karo, `==` nahi.**

This is one of the most common Java interview traps for developers coming from JavaScript.

---

# 1.6 String Immutability

Java `String` **immutable** hai.

Example:

```java
String name = "Rishabh";

name.concat(" Kumar");

System.out.println(name);
```

Output:

```text
Rishabh
```

Why?

Because:

```java
name.concat(" Kumar");
```

existing String ko modify nahi karta.

New String return karta hai.

Correct:

```java
name = name.concat(" Kumar");
```

Now:

```text
Rishabh Kumar
```

---

# 1.7 Why is String Immutable?

String immutability ke several benefits hain:

### 1. Security

Strings frequently use hote hain:

```text
passwords
URLs
file paths
class names
database connection strings
tokens
```

Immutable objects safer and predictable behavior provide karte hain.

### 2. String Pool

Agar Strings immutable na hote, pooled strings ko safely share karna difficult hota.

```java
String a = "Java";
String b = "Java";
```

Dono same object share kar sakte hain because koi bhi us shared object ko modify nahi kar sakta.

### 3. Thread Safety

Immutable objects inherently easier to safely share across threads.

### 4. Hashing

Strings frequently HashMap/HashSet keys ke roop mein use hoti hain.

Immutability ensures hash-related behavior stable rahe.

---

# 1.8 String Concatenation

```java
String firstName = "Rishabh";
String lastName = "Kumar";

String fullName = firstName + " " + lastName;
```

Simple cases ke liye `+` perfectly fine hai.

But repeated concatenation inside loops:

```java
String result = "";

for (int i = 0; i < 10000; i++) {
    result += i;
}
```

performance concern create kar sakta hai because Strings immutable hain.

---

# 1.9 StringBuilder

Repeated modification ke liye:

```java
StringBuilder builder = new StringBuilder();

for (int i = 0; i < 10000; i++) {
    builder.append(i);
}

String result = builder.toString();
```

`StringBuilder` mutable character sequence provide karta hai.

### Rule of thumb

```text
Few concatenations
→ String / +

Repeated concatenation
→ StringBuilder
```

---

# 1.10 StringBuilder vs StringBuffer

|                | StringBuilder    | StringBuffer                        |
| -------------- | ---------------- | ----------------------------------- |
| Mutable        | Yes              | Yes                                 |
| Thread-safe    | No               | Yes                                 |
| Performance    | Generally faster | Generally slower                    |
| Typical choice | Yes              | Only when synchronization is needed |

Modern application code mein generally:

```java
StringBuilder
```

preferred hota hai unless thread-safety requirement specifically ho.

---

# 1.11 Useful String Methods

```java
String value = "Java Developer";
```

### Length

```java
value.length();
```

### Character

```java
value.charAt(0);
```

### Contains

```java
value.contains("Developer");
```

### Starts / Ends

```java
value.startsWith("Java");
value.endsWith("Developer");
```

### Substring

```java
value.substring(0, 4);
```

### Case conversion

```java
value.toUpperCase();
value.toLowerCase();
```

### Trim

```java
value.trim();
```

Modern Java mein whitespace handling ke liye:

```java
value.strip();
```

bhi important hai.

### Split

```java
String[] parts = value.split(" ");
```

---

# 1.12 `isEmpty()` vs `isBlank()`

Important modern Java concept:

```java
String value = "   ";
```

```java
value.isEmpty(); // false
value.isBlank(); // true
```

`isEmpty()` check karta hai length zero hai ya nahi.

`isBlank()` whitespace-only string ko bhi blank consider karta hai.

---

# 1.13 `String`, `StringBuilder`, `StringBuffer`

```text
String
  ↓
Immutable

StringBuilder
  ↓
Mutable + faster + not synchronized

StringBuffer
  ↓
Mutable + synchronized
```

---

# Interview Perspective — String

### Q1. String primitive hai?

No.

### Q2. String immutable kyun hai?

Security, String Pool, thread-safety benefits, stable hashing etc.

### Q3. `==` vs `.equals()`?

```text
==        → reference identity
.equals() → logical/content equality
```

### Q4. String Pool kya hai?

JVM ka mechanism jahan String literals reuse kiye ja sakte hain.

### Q5. StringBuilder kyun?

Repeated string modifications/concatenations efficiently handle karne ke liye.

### Q6. StringBuilder vs StringBuffer?

Builder generally faster/non-synchronized; Buffer synchronized.

### Must Remember

```text
String = class, not primitive
String = immutable
String literals → String Pool
== → reference comparison
.equals() → content comparison
StringBuilder → mutable
StringBuffer → synchronized mutable
isEmpty() ≠ isBlank()
```

---

# 2. OOP — Object-Oriented Programming

Ab Java ka core territory start hota hai.

Java fundamentally **object-oriented programming model** around designed hai.

OOP ke commonly discussed four pillars:

```text
OOP
│
├── Encapsulation
├── Inheritance
├── Polymorphism
└── Abstraction
```

But experienced developer ke liye inko sirf definitions ke form mein yaad karna enough nahi hai.

Important hai samajhna:

> **Java ka object model kaise design hota hai aur ye JavaScript ke prototype-based object model se kaise different hai.**

---

# 2.1 Encapsulation

Encapsulation ka basic idea:

> Data aur us data par operate karne wale behavior ko ek unit ke andar combine karna, aur internal state ko controlled access dena.

Example:

```java
public class BankAccount {

    private double balance;

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

Caller:

```java
BankAccount account = new BankAccount();

account.deposit(1000);

System.out.println(account.getBalance());
```

Caller directly:

```java
account.balance = -5000;
```

nahi kar sakta because `balance` private hai.

---

# 2.2 Encapsulation ≠ Just Getters/Setters

Ye common misconception hai.

Bad design:

```java
private double balance;

public double getBalance() {
    return balance;
}

public void setBalance(double balance) {
    this.balance = balance;
}
```

Agar koi validation/business rule nahi hai, to private field ko blindly public setter dene se encapsulation ka benefit largely weaken ho sakta hai.

Better:

```java
public void withdraw(double amount) {
    if (amount <= 0) {
        throw new IllegalArgumentException("Invalid amount");
    }

    if (amount > balance) {
        throw new IllegalStateException("Insufficient balance");
    }

    balance -= amount;
}
```

Object khud apni state protect kar raha hai.

### Interview insight

> Encapsulation is about **controlling access and protecting invariants**, not merely generating getters and setters.

---

# 2.3 Inheritance

Inheritance:

```text
Parent
  ↓
Child
```

Example:

```java
class Vehicle {
    void start() {
        System.out.println("Vehicle started");
    }
}

class Car extends Vehicle {
    void drive() {
        System.out.println("Car driving");
    }
}
```

Now:

```java
Car car = new Car();

car.start();
car.drive();
```

`Car` inherits `start()` from `Vehicle`.

---

# 2.4 JavaScript vs Java Inheritance

JavaScript:

```javascript
class Car extends Vehicle {}
```

Looks similar.

But underlying models are different.

JavaScript classes are primarily syntactic abstraction over **prototype-based inheritance**.

Java classes are part of Java's class/type system and JVM runtime model.

Java supports:

```text
class → extends → one superclass
```

A Java class cannot extend multiple classes.

But it can implement multiple interfaces:

```java
class Car implements Vehicle, ElectricVehicle {
}
```

---

# 2.5 Polymorphism

Polymorphism literally means:

> One interface/type, multiple implementations/behaviors.

Example:

```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Meow");
    }
}
```

Now:

```java
Animal animal = new Dog();

animal.sound();
```

Output:

```text
Bark
```

Variable type:

```java
Animal
```

Actual object:

```java
Dog
```

Method dispatch runtime object ke according hota hai.

This is **runtime polymorphism**.

---

# 2.6 Compile-Time vs Runtime Polymorphism

### Compile-time

Method overloading:

```java
calculate(int a, int b)
calculate(double a, double b)
```

Compiler decide karta hai kaunsa method call hoga.

### Runtime

Method overriding:

```java
Animal animal = new Dog();

animal.sound();
```

Runtime actual object determine karta hai overridden implementation.

---

# 2.7 Abstraction

Abstraction ka goal:

> Complex implementation details hide karke required behavior expose karna.

Example:

```java
interface PaymentService {
    void pay(double amount);
}
```

Implementations:

```java
class StripePayment implements PaymentService {

    @Override
    public void pay(double amount) {
        // Stripe-specific implementation
    }
}
```

Caller ko implementation details nahi pata:

```java
PaymentService paymentService = new StripePayment();

paymentService.pay(1000);
```

Caller sirf contract jaanta hai.

---

# 2.8 OOP Pillars — Quick View

| Pillar        | Main idea                         |
| ------------- | --------------------------------- |
| Encapsulation | State + controlled access         |
| Inheritance   | Reuse/relationship                |
| Polymorphism  | Same contract, different behavior |
| Abstraction   | Hide implementation complexity    |

---

# 2.9 JavaScript vs Java OOP

| JavaScript                              | Java                                 |
| --------------------------------------- | ------------------------------------ |
| Prototype-based                         | Class/type-based                     |
| Classes available                       | Classes fundamental                  |
| Dynamic typing                          | Static typing                        |
| Prototype chain                         | Class inheritance hierarchy          |
| Private fields `#`                      | Access modifiers + encapsulation     |
| Multiple prototype composition possible | One superclass + multiple interfaces |
| Duck typing common                      | Explicit types/contracts             |

---

# Interview Perspective — OOP

Common questions:

* OOP ke four pillars?
* Encapsulation kya hai?
* Encapsulation vs abstraction?
* Inheritance kab use karna chahiye?
* Polymorphism kya hai?
* Overloading vs overriding?
* Java multiple inheritance of classes kyun support nahi karta?
* Interface multiple implement kyun kar sakte hain?
* Composition vs inheritance?

### Must Remember

```text
Encapsulation → protect/manage state
Abstraction   → hide implementation
Inheritance   → is-a relationship
Polymorphism  → one contract, multiple behavior

Overloading  → compile-time
Overriding   → runtime
```

---

# 3. Classes & Objects

Ab OOP ko actual Java syntax mein samjho.

---

# 3.1 Class

Class ek **blueprint/type definition** hai.

```java
public class User {

    String name;
    String email;

    void login() {
        System.out.println(name + " logged in");
    }
}
```

Class define karti hai:

```text
state
+
behavior
```

---

# 3.2 Object

Object class ka actual instance hai.

```java
User user = new User();
```

Yahan:

```text
User
 ↓
type/class

new User()
 ↓
object

user
 ↓
reference variable
```

Important distinction:

> `user` object nahi hai; `user` object ka reference hold karne wala variable hai.

---

# 3.3 JavaScript Comparison

JavaScript:

```javascript
const user = {
    name: "Rishabh",
    login() {
        console.log("Logged in");
    }
};
```

Java:

```java
User user = new User();
```

Java mein object structure class/type definition se strongly associated hai.

---

# 3.4 Fields

```java
public class User {

    private String name;
    private String email;
}
```

`name` and `email` fields/state represent karte hain.

---

# 3.5 Methods

```java
public void login() {
    System.out.println("User logged in");
}
```

Behavior represent karta hai.

---

# 3.6 Access Modifiers

Java classes/fields/methods ke access ko control karne ke liye:

```text
public
private
protected
default/package-private
```

### `private`

Sirf same class ke andar accessible.

```java
private String password;
```

### `public`

Everywhere accessible, subject to normal access rules.

### `protected`

Same package + subclasses.

### package-private

Agar modifier nahi diya:

```java
String name;
```

to package-private access hota hai.

---

# 3.7 `this`

JavaScript `this` se conceptually familiar hai, but Java mein behavior much more predictable hai.

```java
public class User {

    private String name;

    public void setName(String name) {
        this.name = name;
    }
}
```

Yahan:

```text
this.name
```

current object ka field.

```text
name
```

method parameter.

---

# 3.8 Object Identity

```java
User user1 = new User();
User user2 = new User();
```

Even agar dono ki fields same hain, they're different objects.

```java
user1 == user2
```

→ false.

Object equality ke liye classes commonly `.equals()` implement/override karti hain.

---

# 3.9 `equals()` and `hashCode()`

Ye OOP ke saath closely related interview topic hai.

Suppose:

```java
User u1 = new User("Rishabh");
User u2 = new User("Rishabh");
```

Without overriding equality semantics:

```java
u1.equals(u2)
```

typically identity-based behavior inherit kar sakta hai from `Object`.

Agar logical equality define karni hai, class ko appropriately override karna hota hai:

```java
@Override
public boolean equals(Object obj) {
    // equality logic
}

@Override
public int hashCode() {
    // consistent hash
}
```

### Critical contract

If:

```text
a.equals(b) == true
```

then:

```text
a.hashCode() == b.hashCode()
```

hona chahiye.

But reverse necessarily true nahi hai.

Ye `HashMap` / `HashSet` interviews mein very common topic hai.

---

# 3.10 Composition

Modern Java design mein inheritance ke comparison mein **composition** bahut important hai.

Example:

```java
class Engine {
    void start() {}
}

class Car {
    private Engine engine;

    Car() {
        this.engine = new Engine();
    }
}
```

Car:

```text
has-a Engine
```

Instead of:

```text
Car is-a Engine
```

### Rule

```text
Inheritance → is-a
Composition → has-a
```

Production design mein often:

> **Prefer composition over inheritance**

because inheritance tight coupling create kar sakti hai.

---

# Interview Perspective — Classes/Objects

Important questions:

* Class vs object?
* Object reference kya hota hai?
* `this` kya hai?
* `==` vs `.equals()`?
* `equals()` and `hashCode()` relationship?
* Access modifiers?
* Composition vs inheritance?
* `is-a` vs `has-a`?

### Must Remember

```text
Class → blueprint/type
Object → class ka instance
Reference → object ko point karta hai
this → current object
private → encapsulation
is-a → inheritance
has-a → composition
equals() → logical equality
hashCode() → hashing contract
```

---

# 4. Constructors

Constructor Java ka very important concept hai.

Constructor ka main purpose:

> **Object ko valid initial state mein initialize karna.**

---

# 4.1 Basic Constructor

```java
public class User {

    private String name;

    public User(String name) {
        this.name = name;
    }
}
```

Object:

```java
User user = new User("Rishabh");
```

Constructor automatically invoke hota hai.

---

# 4.2 Constructor ka Name

Constructor ka naam class ke same hota hai:

```java
class User {

    User() {
    }
}
```

Return type nahi hota.

This is **not** a method:

```java
User() {}
```

Correct constructor.

---

# 4.3 Default Constructor vs No-Arg Constructor

Ye interview mein frequently confuse kiya jata hai.

Agar tum koi constructor define nahi karte:

```java
class User {
}
```

compiler automatically ek **default constructor** provide kar sakta hai.

Conceptually:

```java
User() {
}
```

But agar tum explicitly constructor define kar dete ho:

```java
class User {

    User(String name) {
    }
}
```

to compiler automatically no-arg constructor nahi deta.

Therefore:

```java
new User();
```

compile nahi karega.

---

# 4.4 Constructor Overloading

Multiple constructors possible:

```java
public class User {

    private String name;
    private String email;

    public User() {
    }

    public User(String name) {
        this.name = name;
    }

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
}
```

This is constructor overloading.

---

# 4.5 Constructor Chaining

Constructor ke andar:

```java
this(...)
```

use karke same class ke another constructor ko call kar sakte ho.

```java
public class User {

    private String name;
    private String role;

    public User() {
        this("Unknown", "USER");
    }

    public User(String name) {
        this(name, "USER");
    }

    public User(String name, String role) {
        this.name = name;
        this.role = role;
    }
}
```

Benefit:

> Duplicate initialization logic avoid hota hai.

---

# 4.6 `this()` Rule

`this()` constructor call:

> Constructor ke **first statement** hona chahiye.

Correct:

```java
User() {
    this("Unknown");
}
```

Incorrect:

```java
User() {
    System.out.println("Creating user");
    this("Unknown"); // invalid
}
```

---

# 4.7 Parent Constructor — `super()`

Inheritance ke case mein:

```java
class Vehicle {

    Vehicle(String type) {
        System.out.println(type);
    }
}

class Car extends Vehicle {

    Car() {
        super("Car");
    }
}
```

`super(...)` parent constructor call karta hai.

---

# 4.8 `this()` vs `super()`

| `this()`                                | `super()`                               |
| --------------------------------------- | --------------------------------------- |
| Same class constructor                  | Parent class constructor                |
| Constructor chaining                    | Parent initialization                   |
| First statement                         | First statement                         |
| Cannot both be used in same constructor | Cannot both be used in same constructor |

Because both first statement hone chahiye.

---

# 4.9 Constructor vs Method

| Constructor                                 | Method                              |
| ------------------------------------------- | ----------------------------------- |
| Initializes object                          | Performs behavior                   |
| Same name as class                          | Any valid name                      |
| No return type                              | Return type required/`void`         |
| Automatically called during object creation | Explicitly invoked                  |
| Cannot be inherited                         | Methods can be inherited/overridden |
| Cannot be overridden                        | Can be overridden                   |

---

# 4.10 Private Constructor

Java allows:

```java
class DatabaseConnection {

    private DatabaseConnection() {
    }
}
```

Why?

To prevent external code from directly creating instances.

Used in certain designs such as:

* utility classes
* factory patterns
* controlled instance creation

Example utility class:

```java
public final class MathUtils {

    private MathUtils() {
        throw new UnsupportedOperationException("Utility class");
    }

    public static int add(int a, int b) {
        return a + b;
    }
}
```

Then:

```java
MathUtils.add(10, 20);
```

No object creation required.

---

# Interview Perspective — Constructors

### Important questions

1. Constructor kya hota hai?
2. Constructor ka return type hota hai?
3. Default constructor kya hai?
4. Constructor overloading?
5. `this()` vs `super()`?
6. `this()` first statement kyun hona chahiye?
7. Agar constructor manually define kar diya to compiler-generated constructor ka kya hota hai?
8. Private constructor ka use?
9. Constructor inherit/override ho sakta hai?

### Must Remember

```text
Constructor → object initialization
same name as class
no return type
called during new
constructors can be overloaded
this() → same class constructor
super() → parent constructor
this()/super() → first statement
constructors are not inherited
constructors cannot be overridden
```

---

# 5. Abstract Classes

Ab hum OOP ke abstraction part par aate hain.

Abstract class ka purpose:

> **Common state/behavior define karna while forcing subclasses to provide certain behavior.**

---

# 5.1 Basic Example

```java
public abstract class Payment {

    protected double amount;

    public Payment(double amount) {
        this.amount = amount;
    }

    public abstract void processPayment();

    public void printReceipt() {
        System.out.println("Receipt generated");
    }
}
```

Yahan:

```java
public abstract void processPayment();
```

abstract method hai.

Iska implementation subclass provide karega.

---

# 5.2 Concrete Subclass

```java
public class CreditCardPayment extends Payment {

    public CreditCardPayment(double amount) {
        super(amount);
    }

    @Override
    public void processPayment() {
        System.out.println("Processing credit card payment");
    }
}
```

Another:

```java
public class UpiPayment extends Payment {

    public UpiPayment(double amount) {
        super(amount);
    }

    @Override
    public void processPayment() {
        System.out.println("Processing UPI payment");
    }
}
```

Now:

```java
Payment payment = new UpiPayment(1000);

payment.processPayment();
payment.printReceipt();
```

This gives us:

```text
Abstraction
+
Inheritance
+
Polymorphism
```

all together.

---

# 5.3 Abstract Class ka Object?

Directly:

```java
Payment payment = new Payment(1000);
```

invalid.

Abstract class instantiate nahi kar sakte.

But:

```java
Payment payment = new UpiPayment(1000);
```

valid.

Because reference type abstract class ho sakta hai, actual object concrete subclass ka hai.

---

# 5.4 Abstract Class Can Have Concrete Methods

Important:

Abstract class ka matlab ye nahi ki **all methods abstract hone chahiye**.

Example:

```java
abstract class Payment {

    abstract void process();

    void generateReceipt() {
        System.out.println("Receipt generated");
    }
}
```

Allowed.

So abstract class can contain:

```text
abstract methods
+
concrete methods
+
fields
+
constructors
```

---

# 5.5 Abstract Class Can Have Constructor

Ye beginners ko often surprising lagta hai.

```java
abstract class Payment {

    protected double amount;

    Payment(double amount) {
        this.amount = amount;
    }
}
```

Abstract class ka direct object nahi bana sakte, but constructor subclass object creation ke time execute ho sakta hai.

```java
class UpiPayment extends Payment {

    UpiPayment(double amount) {
        super(amount);
    }
}
```

Object:

```java
new UpiPayment(1000);
```

During construction:

```text
UpiPayment constructor
        ↓
Payment constructor
        ↓
UpiPayment object initialization
```

---

# 5.6 Abstract Method Rules

Abstract method:

```java
abstract void process();
```

ka body nahi hota.

Wrong:

```java
abstract void process() {
}
```

Also abstract method ko subclass generally implement karega, unless subclass itself abstract ho.

---

# 5.7 Abstract Class vs Concrete Class

| Abstract Class                      | Concrete Class                 |
| ----------------------------------- | ------------------------------ |
| `abstract` keyword                  | No `abstract`                  |
| Directly instantiate nahi kar sakte | Instantiate kar sakte          |
| Abstract methods ho sakte hain      | Abstract methods nahi ho sakte |
| Constructors ho sakte hain          | Constructors ho sakte hain     |
| Fields ho sakte hain                | Fields ho sakte hain           |
| Concrete methods ho sakte hain      | Concrete methods               |

---

# 5.8 Abstract Class vs Interface

Ye next-level interview question hai.

### Abstract class

```java
abstract class Payment {

    protected double amount;

    Payment(double amount) {
        this.amount = amount;
    }

    abstract void process();

    void receipt() {
        System.out.println("Receipt");
    }
}
```

### Interface

```java
interface Payment {

    void process();
}
```

Modern Java interfaces much more capable hain than older Java versions—they can have `default`, `static`, and private methods—but they still represent a different abstraction/contract model.

---

## Comparison

| Abstract Class                 | Interface                                          |
| ------------------------------ | -------------------------------------------------- |
| `extends`                      | `implements`                                       |
| One superclass only            | Multiple interfaces implement kar sakte hain       |
| Instance fields allowed        | Instance state generally not                       |
| Constructors allowed           | Constructors nahi                                  |
| Abstract + concrete methods    | Abstract + default/static/private methods possible |
| Shared state + behavior        | Contract/capability                                |
| Strong base-class relationship | Often loose contract                               |

---

# 5.9 When Should You Use Abstract Class?

Suppose:

```text
Payment
├── CreditCardPayment
├── UpiPayment
└── NetBankingPayment
```

Agar sab payment types ke paas:

* common state
* common validation
* common behavior
* common lifecycle

hai, abstract class useful ho sakti hai.

Example:

```java
abstract class Payment {

    protected BigDecimal amount;

    protected Payment(BigDecimal amount) {
        this.amount = amount;
    }

    public void validate() {
        // common validation
    }

    public abstract void process();
}
```

Subclasses:

```java
class UpiPayment extends Payment {
    @Override
    public void process() {
        // UPI specific
    }
}
```

---

# 5.10 When Interface Better Hai?

Agar requirement primarily contract/capability express karna hai:

```java
interface Payable {
    void pay();
}
```

Different unrelated classes implement kar sakti hain:

```java
class Invoice implements Payable
class Subscription implements Payable
class Order implements Payable
```

Yahan inheritance relationship necessarily nahi hai.

---

# 5.11 JavaScript Developer Perspective

JavaScript mein tum commonly composition, objects, classes, mixins, duck typing etc. use kar sakte ho.

Java mein static type system ki wajah se:

```java
Payment payment
```

ek meaningful compile-time contract banata hai.

For example:

```java
Payment payment = new UpiPayment(1000);
```

Compiler guarantee karta hai ki `UpiPayment` `Payment` contract/type ke according compatible hai.

---

# Interview Perspective — Abstract Classes

### Q1. Abstract class ka object bana sakte hain?

No.

### Q2. Abstract class constructor ho sakta hai?

Yes.

### Q3. Abstract class mein concrete methods ho sakte hain?

Yes.

### Q4. Abstract class mein fields ho sakti hain?

Yes.

### Q5. Abstract class ko subclass karne ke baad?

Agar subclass concrete hai, to inherited abstract methods implement karne honge.

### Q6. Abstract class vs interface?

Core distinction:

```text
Abstract class
→ shared state + shared behavior + abstraction

Interface
→ contract/capability
```

Modern Java mein interfaces powerful hain, so distinction sirf “interface has methods, abstract class has implementation” tak limited nahi hai.

### Must Remember

```text
abstract class → cannot instantiate
can have constructors
can have fields
can have concrete methods
can have abstract methods

abstract method → no implementation body

subclass must implement abstract methods
unless subclass itself abstract
```

---

# 🔥 String → OOP → Classes → Constructors → Abstract Classes: Connected Mental Model

Ab in concepts ko isolated topics ki tarah nahi, ek system ki tarah dekho.

Suppose hum payment system bana rahe hain:

```text
                Payment
             abstract class
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
 CreditCard       UPI      NetBanking
```

`Payment`:

```java
abstract class Payment {

    protected BigDecimal amount;

    protected Payment(BigDecimal amount) {
        this.amount = amount;
    }

    public void validate() {
        // common validation
    }

    public abstract void process();
}
```

UPI:

```java
class UpiPayment extends Payment {

    public UpiPayment(BigDecimal amount) {
        super(amount);
    }

    @Override
    public void process() {
        System.out.println("Processing UPI payment");
    }
}
```

Caller:

```java
Payment payment =
    new UpiPayment(new BigDecimal("999.00"));

payment.validate();
payment.process();
```

Is single example mein:

```text
class
    ↓
object
    ↓
constructor
    ↓
inheritance
    ↓
abstract class
    ↓
polymorphism
    ↓
encapsulation
```

almost sab connect ho raha hai.

---

# 🧠 JavaScript Developer ke liye Biggest Differences

## 1. String equality

JavaScript:

```javascript
"Java" === "Java"
```

Java:

```java
a.equals(b)
```

**`==` se String compare mat karo.**

---

## 2. String immutable

Java:

```java
String name = "Rishabh";

name.toUpperCase();
```

`name` change nahi hota.

```java
name = name.toUpperCase();
```

New value assign karni hoti hai.

---

## 3. Class is a real type

JavaScript:

```javascript
const user = {};
```

Java:

```java
User user = new User();
```

Java mein class/type system much stronger hai.

---

## 4. Constructor is not a method

Java:

```java
User(String name) {
    this.name = name;
}
```

No return type.

Constructor ka purpose initialization hai.

---

## 5. `==` ka meaning

JavaScript mein `==` coercion karta hai aur `===` strict equality hai.

Java mein:

```java
==
```

reference/value identity context ke according primitive values compare karta hai, while object references ke case mein **same object reference** check karta hai.

So JavaScript wala mental model directly transfer mat karo.

---

## 6. Encapsulation is stronger

Java:

```java
private BigDecimal balance;
```

aur controlled methods:

```java
deposit()
withdraw()
```

Object apni invariants protect kar sakta hai.

---

## 7. Inheritance ko default design strategy mat samjho

Java mein technically:

```java
class A extends B
```

easy hai.

But professional design mein often:

```text
Composition > unnecessary inheritance
```

prefer kiya jata hai.

---

## 8. Abstract class = reusable base + incomplete behavior

Abstract class sirf “class jiska object nahi bana sakte” nahi hai.

Better mental model:

> **A partially implemented base abstraction that can enforce a common contract while sharing state and behavior.**

---

# 🎯 Final Interview Cheat Sheet

### String

```text
String = immutable class
String Pool = literal reuse
== = reference identity
equals() = logical equality
StringBuilder = mutable string building
StringBuffer = synchronized mutable alternative
```

### OOP

```text
Encapsulation → protect state
Abstraction → hide implementation
Inheritance → is-a
Polymorphism → multiple implementations
Composition → has-a
```

### Classes / Objects

```text
Class → type/blueprint
Object → instance
Reference → points to object
this → current object
private → encapsulation
equals/hashCode → logical equality + hashing contract
```

### Constructors

```text
Constructor → initialization
same name as class
no return type
can overload
this() → same class constructor
super() → parent constructor
constructors cannot be overridden
```

### Abstract Classes

```text
cannot instantiate
can have constructor
can have fields
can have concrete methods
can have abstract methods
subclass must implement abstract methods
unless subclass is abstract
```

### ⭐ Most Important Interview Traps

```text
1. String is NOT primitive
2. String is immutable
3. String == vs equals()
4. Java is pass-by-value
5. Constructor is NOT a method
6. Constructors are not inherited/overridden
7. Abstract class can have constructor
8. Abstract class can have concrete methods
9. Overloading ≠ overriding
10. Inheritance ≠ composition
11. Array.length vs Collection.size()
12. int ≠ Integer
```
