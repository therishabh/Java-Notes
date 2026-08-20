
# Java Notes

## 1. Data Types

### 1.1 Concept

Java **statically typed** language hai.

JavaScript mein:

```javascript
let age = 30;
age = "thirty"; // valid
```

Java mein:

```java
int age = 30;
age = "thirty"; // Compile-time error
```

Java mein variable ka type compile time par known hota hai aur generally change nahi hota.

Java ke data types do major categories mein divide hote hain:

```text
Data Types
│
├── Primitive Types
│   ├── byte
│   ├── short
│   ├── int
│   ├── long
│   ├── float
│   ├── double
│   ├── char
│   └── boolean
│
└── Reference Types
    ├── String
    ├── Arrays
    ├── Classes
    ├── Interfaces
    ├── Enums
    └── Objects
```

---

## 1.2 Primitive Data Types

| Type      |          Size | Example                     | Typical Use             |
| --------- | ------------: | --------------------------- | ----------------------- |
| `byte`    |         8-bit | `byte b = 100;`             | Small numeric data      |
| `short`   |        16-bit | `short s = 1000;`           | Rare                    |
| `int`     |        32-bit | `int age = 30;`             | Default integer         |
| `long`    |        64-bit | `long id = 123456789L;`     | Large integers          |
| `float`   |        32-bit | `float price = 10.5f;`      | Decimal, less precision |
| `double`  |        64-bit | `double salary = 50000.50;` | Default floating point  |
| `char`    |        16-bit | `char grade = 'A';`         | Single UTF-16 code unit |
| `boolean` | JVM-dependent | `boolean active = true;`    | `true/false`            |

### Most important practical point

Professional Java development mein tum frequently use karoge:

```java
int
long
double
boolean
char
```

Aur business applications mein:

```java
String
BigDecimal
Integer
Long
Boolean
```

etc.

---

## 1.3 `int` vs `long`

Java mein integer literals by default `int` hote hain.

```java
int count = 100;
long userId = 100L;
```

`L` explicitly batata hai ki literal `long` hai.

```java
long value = 10000000000L;
```

Without `L`:

```java
long value = 10000000000; // compilation error
```

because literal itself `int` range se bahar hai.

---

## 1.4 Floating Point

```java
double price = 99.99;
float discount = 10.5f;
```

`float` ke literal ke end mein `f` lagana padta hai.

Real-world financial calculations mein generally:

```java
double
```

use nahi karna chahiye because floating-point precision issues ho sakte hain.

Instead:

```java
BigDecimal price = new BigDecimal("99.99");
```

For example:

```java
BigDecimal price = new BigDecimal("19.99");
BigDecimal quantity = new BigDecimal("3");

BigDecimal total = price.multiply(quantity);
```

### Interview point

> **Money ke liye `BigDecimal` prefer karo, `double` nahi.**

---

# 1.5 `char` — JavaScript Developer ke liye important difference

Java:

```java
char grade = 'A';
```

Single quotes → `char`

Double quotes:

```java
String name = "Rishabh";
```

→ `String`

JavaScript mein:

```javascript
const value = 'A';
const name = "Rishabh";
```

dono strings hain.

Java mein:

```java
'A'   // char
"A"   // String
```

This is a very common interview/basic Java gotcha.

---

# 1.6 Reference Types

Primitive:

```java
int age = 30;
```

Reference type:

```java
String name = "Rishabh";
```

```java
User user = new User();
```

Conceptually:

```text
Primitive
variable ──> actual value

Reference
variable ──> reference ──> object
```

JavaScript developers ke liye important point:

**Java mein everything object nahi hai.**

JavaScript mein primitive values bhi hoti hain, but Java mein primitive system much more explicit hai.

---

# 1.7 `String` Primitive nahi hai

Ye important interview question hai:

> Is String a primitive data type in Java?

**No.**

```java
String name = "Rishabh";
```

`String` ek class hai.

Isliye:

```java
String
```

reference type hai.

---

# 1.8 `null`

Reference types:

```java
String name = null;
User user = null;
```

possible hain.

Primitive:

```java
int age = null; // invalid
```

because primitive types `null` hold nahi kar sakte.

---

# 1.9 JavaScript vs Java

| JavaScript                                    | Java                            |
| --------------------------------------------- | ------------------------------- |
| Dynamically typed                             | Statically typed                |
| `let x = 10`                                  | `int x = 10`                    |
| Variable type runtime par change ho sakta hai | Variable ka declared type fixed |
| Number single `number` type                   | Multiple numeric types          |
| String primitive hai                          | `String` class hai              |
| `null` + `undefined`                          | `null`, but no `undefined`      |
| Objects/reference semantics                   | Primitive + reference types     |
| Type errors often runtime                     | Many type errors compile time   |

### Biggest mental-model change

JavaScript:

> “Variable mein abhi kya value hai?”

Java:

> “Variable ka declared type kya hai, aur us type ke according kya operations allowed hain?”

---

# 1.10 Interview Perspective

### Common questions

**Q1. Java mein kitne primitive data types hain?**

8:

```text
byte
short
int
long
float
double
char
boolean
```

**Q2. String primitive hai?**

No.

**Q3. `char` kitne bits ka hota hai?**

16-bit.

**Q4. `int` aur `Integer` mein difference?**

`int` primitive hai.

`Integer` wrapper/reference type hai.

```java
int age = 30;

Integer ageObject = 30;
```

Wrapper classes ko Arrays, Collections, generics etc. ke saath frequently use kiya jata hai.

**Q5. Money ke liye `double` kyun avoid karte hain?**

Floating-point precision problems ki wajah se. `BigDecimal` preferred hai.

### Must Remember

```text
Java = statically typed
8 primitive types
String = reference type
char != String
int != Integer
long literals → L
float literals → f
money → BigDecimal
primitive cannot be null
reference can be null
```

---

# 2. Arrays

## 2.1 Concept

JavaScript:

```javascript
const numbers = [10, 20, 30];
```

Java:

```java
int[] numbers = {10, 20, 30};
```

Ya:

```java
int[] numbers = new int[3];

numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;
```

Java Array ki **fixed length** hoti hai.

Ye JavaScript Array se major difference hai.

---

# 2.2 Array Declaration

Recommended syntax:

```java
int[] numbers;
```

Alternative:

```java
int numbers[];
```

Modern Java code mein generally:

```java
int[] numbers;
```

prefer kiya jata hai.

---

# 2.3 Initialization

```java
int[] numbers = {10, 20, 30, 40};
```

Length:

```java
numbers.length
```

Notice:

Java:

```java
numbers.length
```

JavaScript:

```javascript
numbers.length
```

Same-looking API, but Java mein `length` **field** hai, method nahi.

So:

```java
numbers.length() // wrong
```

---

# 2.4 Fixed Size

```java
int[] numbers = {10, 20, 30};
```

Is array mein fourth element directly add nahi kar sakte.

JavaScript:

```javascript
numbers.push(40);
```

Java:

```java
numbers.push(40); // doesn't exist
```

Agar dynamic collection chahiye:

```java
List<Integer> numbers = new ArrayList<>();
```

Then:

```java
numbers.add(40);
```

### Mental model

```text
Java Array
    ↓
Fixed-size data structure

Java List
    ↓
Dynamic collection
```

---

# 2.5 Arrays of Objects

```java
User[] users = new User[3];

users[0] = new User("Rishabh");
users[1] = new User("Amit");
```

Important:

```java
User[] users = new User[3];
```

objects create nahi karta.

Ye sirf references ka array create karta hai.

Initially:

```text
users
 ├── null
 ├── null
 └── null
```

Then:

```java
users[0] = new User("Rishabh");
```

ke baad:

```text
users
 ├── User object
 ├── null
 └── null
```

Ye Java memory model samajhne ke liye important hai.

---

# 2.6 Multidimensional Arrays

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};
```

Access:

```java
matrix[0][1]
```

Result:

```text
2
```

Java ka multidimensional array technically **array of arrays** hai.

Isliye jagged arrays possible hain:

```java
int[][] data = {
    {1, 2},
    {3, 4, 5},
    {6}
};
```

Ye valid Java hai.

---

# 2.7 Array Traversal

Traditional:

```java
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```

Enhanced `for`:

```java
for (int number : numbers) {
    System.out.println(number);
}
```

Modern Java mein jab index ki zarurat nahi hai:

```java
for (int number : numbers)
```

cleaner approach hai.

---

# 2.8 JavaScript vs Java

| Feature             | JavaScript Array | Java Array                  |
| ------------------- | ---------------- | --------------------------- |
| Size                | Dynamic          | Fixed                       |
| Mixed types         | Possible         | Normally same declared type |
| `push()`            | Yes              | No                          |
| `pop()`             | Yes              | No                          |
| Length              | `array.length`   | `array.length`              |
| Generics            | No               | Collections use generics    |
| Dynamic alternative | Array            | `List`                      |

---

# 2.9 `Array` vs `ArrayList`

Ye interview mein bahut important hai.

### Array

```java
int[] numbers = new int[10];
```

Fixed size.

### ArrayList

```java
List<Integer> numbers = new ArrayList<>();
```

Dynamic size.

```java
numbers.add(10);
numbers.add(20);
numbers.remove(0);
```

Typical business application code mein collections zyada common hongi.

### Important

`ArrayList<Integer>` mein `Integer` use hota hai, `int` nahi.

```java
List<int> numbers; // invalid
```

because Java generics primitives support nahi karte.

Instead:

```java
List<Integer> numbers;
```

Yahan **autoboxing** help karta hai:

```java
numbers.add(10);
```

Java automatically:

```text
int → Integer
```

convert kar deta hai.

---

# 2.10 Common Mistakes

### Mistake 1

```java
int[] numbers = new int[3];

numbers[3] = 10;
```

Invalid because indexes:

```text
0
1
2
```

Last index:

```java
numbers.length - 1
```

Runtime par:

```text
ArrayIndexOutOfBoundsException
```

---

### Mistake 2

```java
int[] numbers;
System.out.println(numbers);
```

Local variable ko initialize kiye bina use nahi kar sakte.

---

### Mistake 3

```java
int[] numbers = null;
numbers.length;
```

→ `NullPointerException`

---

# Interview Perspective

### Common questions

**Q1. Java array fixed size hai?**

Yes.

**Q2. Array ka size runtime par change kar sakte ho?**

Existing array ka size change nahi hota. New array create karna padega.

**Q3. Java Array aur ArrayList mein difference?**

Core difference:

```text
Array     → fixed-size
ArrayList → dynamic-size collection
```

**Q4. `int[]` aur `Integer[]` difference?**

```java
int[]     // primitives
Integer[] // references
```

`Integer[]` ke elements initially `null` ho sakte hain.

`int[]` ke elements default `0` hote hain.

### Must Remember

```text
Array = fixed-size
index starts at 0
length = field
last index = length - 1
ArrayIndexOutOfBoundsException = invalid index
Object array initially contains null references
dynamic collection → List / ArrayList
generics don't support primitives
```

---

# 3. Functions / Methods

JavaScript developer ke liye yahan terminology change hoti hai.

JavaScript:

```javascript
function calculateTotal(price, quantity) {
    return price * quantity;
}
```

Java:

```java
int calculateTotal(int price, int quantity) {
    return price * quantity;
}
```

But Java mein functions generally **class ke andar methods** hote hain.

Java mein standalone top-level function nahi hota.

---

# 3.1 Basic Method

```java
public int calculateTotal(int price, int quantity) {
    return price * quantity;
}
```

Breakdown:

```text
public
  ↓
access modifier

int
  ↓
return type

calculateTotal
  ↓
method name

(int price, int quantity)
  ↓
parameters

return price * quantity;
  ↓
method body
```

---

# 3.2 `void`

Agar method kuch return nahi karta:

```java
public void logUser(String username) {
    System.out.println(username);
}
```

JavaScript mein:

```javascript
function logUser(username) {
    console.log(username);
}
```

Java mein explicit:

```java
void
```

likhna required hai.

---

# 3.3 Parameters are Statically Typed

```java
public double calculateDiscount(
    double price,
    double percentage
) {
    return price * percentage / 100;
}
```

Call:

```java
calculateDiscount(1000, 10);
```

Wrong type:

```java
calculateDiscount("1000", 10);
```

compile-time issue.

---

# 3.4 `static` Methods

Java mein:

```java
public static int add(int a, int b) {
    return a + b;
}
```

`static` method class se associated hota hai, particular object se nahi.

Call:

```java
Math.max(10, 20);
```

`Math.max()` static method ka example hai.

---

# 3.5 Instance Method

```java
User user = new User();

user.getName();
```

`getName()` instance method hai.

Conceptually:

```text
static method
Class.method()

instance method
object.method()
```

---

# 3.6 Method Overloading

Java mein same method name multiple times define kar sakte ho, provided parameter list different ho.

```java
public int calculate(int a, int b) {
    return a + b;
}

public double calculate(double a, double b) {
    return a + b;
}
```

Ya:

```java
public int calculate(int a, int b, int c) {
    return a + b + c;
}
```

This is **method overloading**.

---

# 3.7 JavaScript vs Java

JavaScript mein traditionally:

```javascript
function calculate(a, b) {}
function calculate(a, b, c) {}
```

same scope mein actual function overloading Java-style nahi hoti. Later declaration generally earlier one ko replace karegi.

Java:

```java
calculate(int, int)
calculate(int, int, int)
calculate(double, double)
```

valid overloads hain.

---

# 3.8 Java Pass-by-Value

Ye **extremely important interview topic** hai.

Java is **always pass-by-value**.

Primitive:

```java
void update(int value) {
    value = 100;
}

int x = 10;
update(x);

System.out.println(x);
```

Output:

```text
10
```

because copy pass hui.

---

## Object ke case mein confusion

```java
void update(User user) {
    user.setName("John");
}
```

Agar:

```java
User user = new User("Rishabh");

update(user);
```

to object mutate ho sakta hai.

But iska matlab ye nahi ki Java pass-by-reference hai.

Actually:

```text
reference ki copy pass hoti hai
```

Conceptually:

```text
original variable
      │
      ▼
   User Object
      ▲
      │
 copied reference
```

Method object ko mutate kar sakta hai because copied reference same object ko point kar raha hai.

Lekin reference ko replace karne se original reference change nahi hota:

```java
void update(User user) {
    user = new User("John");
}
```

Original caller ka reference same rahega.

### Interview line

> **Java is always pass-by-value; for objects, the value being passed is a copy of the reference.**

Ye line yaad rakhna.

---

# 3.9 Varargs

Java mein variable number of arguments:

```java
public int sum(int... numbers) {
    int total = 0;

    for (int number : numbers) {
        total += number;
    }

    return total;
}
```

Call:

```java
sum(1, 2, 3);
sum(10, 20);
sum(1, 2, 3, 4, 5);
```

Internally `numbers` array ki tarah behave karta hai.

---

# 3.10 Methods vs JavaScript Functions

| JavaScript                   | Java                                                 |
| ---------------------------- | ---------------------------------------------------- |
| Function                     | Method generally class ke andar                      |
| Dynamic parameters           | Statically typed parameters                          |
| Return type implicit         | Return type explicitly declared                      |
| Functions first-class values | Methods themselves aren't equivalent to JS functions |
| Closures common              | Lambdas provide functional style                     |
| Default parameters supported | Different approach; overloads etc.                   |
| Rest parameter `...args`     | Varargs `...args`                                    |

---

# Common Mistakes

### Wrong return type

```java
public int getName() {
    return "Rishabh";
}
```

Invalid.

Return type must match.

---

### Missing return

```java
public int calculate() {
    // no return
}
```

Compilation error.

---

### Confusing overload with override

```text
Overloading
→ same class/name
→ different parameters

Overriding
→ inheritance
→ subclass replaces parent behavior
```

Don't mix these concepts.

---

# Interview Perspective

Common questions:

1. Java mein standalone function possible hai?

   * No, methods belong to classes/interfaces/etc.

2. Java pass-by-value hai ya reference?

   * Always pass-by-value.

3. Method overloading kya hai?

4. `static` method kya hota hai?

5. `void` kya represent karta hai?

6. Varargs internally kis form mein behave karta hai?

7. Overloading aur overriding difference?

### Must Remember

```text
Java has methods, not standalone top-level functions.
Return type explicitly declared.
Parameters statically typed.
Java is always pass-by-value.
Object case = copy of reference passed.
static → class-level
instance method → object-level
overloading → compile-time polymorphism concept
```

---

# 4. Type Conversion

JavaScript mein type conversion extremely flexible hai:

```javascript
Number("123");
String(123);
Boolean(1);
```

Aur implicit coercion bhi hoti hai:

```javascript
"10" + 5
```

Java mein implicit conversion much more controlled hai.

---

# 4.1 Widening Conversion

Smaller compatible numeric type → larger type.

Example:

```java
int count = 100;
long value = count;
```

Allowed.

Another:

```java
int count = 100;
double value = count;
```

Allowed.

Conceptually:

```text
byte
 ↓
short
 ↓
int
 ↓
long
 ↓
float
 ↓
double
```

Though exact conversion relationships have nuances, interview level par basic widening chain important hai.

---

# 4.2 Narrowing Conversion

Large type → smaller type.

Explicit cast required.

```java
double price = 99.99;

int value = (int) price;
```

Result:

```text
99
```

Decimal truncate ho gaya.

Ye rounding nahi hai.

```java
(int) 99.99
```

→ `99`

---

# 4.3 Data Loss

```java
long value = 10_000_000_000L;

int result = (int) value;
```

Potential overflow/data loss.

Isliye narrowing conversion carefully use karna chahiye.

---

# 4.4 `char` Conversion

```java
char letter = 'A';

int code = letter;
```

`code` becomes:

```text
65
```

Because Java `char` UTF-16 code unit represent karta hai.

Reverse:

```java
int code = 66;

char letter = (char) code;
```

Result:

```text
B
```

---

# 4.5 String → Number

JavaScript:

```javascript
Number("123")
```

Java:

```java
int value = Integer.parseInt("123");
```

Long:

```java
long value = Long.parseLong("123456");
```

Double:

```java
double value = Double.parseDouble("99.5");
```

Invalid input:

```java
Integer.parseInt("abc");
```

throws:

```text
NumberFormatException
```

---

# 4.6 Number → String

```java
int value = 100;

String text = String.valueOf(value);
```

Alternative:

```java
String text = Integer.toString(value);
```

Modern/general-purpose code mein:

```java
String.valueOf(value)
```

common hai.

---

# 4.7 String → Boolean

```java
boolean active = Boolean.parseBoolean("true");
```

Important gotcha:

```java
Boolean.parseBoolean("abc")
```

returns:

```text
false
```

It does **not** throw `NumberFormatException`.

---

# 4.8 JavaScript vs Java

| Scenario               | JavaScript                  | Java                      |
| ---------------------- | --------------------------- | ------------------------- |
| `"123"` → number       | `Number("123")`             | `Integer.parseInt("123")` |
| number → string        | `String(123)`               | `String.valueOf(123)`     |
| implicit coercion      | Common                      | Very limited              |
| `int → double`         | Number same underlying type | Widening conversion       |
| `double → int`         | Number remains number       | Explicit cast             |
| invalid numeric string | `NaN`                       | `NumberFormatException`   |

---

# 4.9 Autoboxing / Unboxing

Java mein primitive aur wrapper ke beech automatic conversion ho sakta hai.

```java
int value = 10;

Integer boxed = value;
```

Primitive → wrapper:

```text
Autoboxing
```

Wrapper → primitive:

```java
Integer value = 10;

int result = value;
```

This is:

```text
Unboxing
```

Collections ke context mein ye bahut important hai:

```java
List<Integer> numbers = new ArrayList<>();

numbers.add(10);
```

`10` primitive literal hai but Java automatically `Integer` bana deta hai.

---

# Common Gotchas

## Integer overflow

```java
int max = Integer.MAX_VALUE;

int result = max + 1;
```

Result positive nahi rahega; overflow ho jayega.

---

## `Integer` vs `int`

```java
Integer value = null;
```

valid.

```java
int value = null;
```

invalid.

Unboxing:

```java
Integer value = null;

int x = value;
```

→ `NullPointerException`

Ye production bugs/interview questions dono mein important hai.

---

# Interview Perspective

Important questions:

* Widening vs narrowing conversion?
* Explicit casting kya hai?
* `int` ko `double` mein convert karna?
* `double` ko `int` mein?
* `String` ko integer mein kaise convert karoge?
* `Integer.parseInt()` invalid input par kya karta hai?
* Autoboxing/unboxing kya hai?
* `Integer null` ko `int` mein unbox karne par kya hoga?

### Must Remember

```text
Widening → usually implicit
Narrowing → explicit cast
double → int can lose decimal
String → int → Integer.parseInt()
int → String → String.valueOf()
primitive ↔ wrapper → boxing/unboxing
null wrapper unboxing → NPE
Java does NOT behave like JS coercion
```

---

# 5. Loops

Tumhare liye basic loop mechanics skip karte hue Java-specific differences par focus karte hain.

Java mein common loops:

```text
for
while
do-while
enhanced for
```

---

# 5.1 Traditional `for`

```java
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}
```

JavaScript se syntax almost same hai.

---

# 5.2 Enhanced `for`

Java ka important construct:

```java
for (String user : users) {
    System.out.println(user);
}
```

Equivalent mental model:

```text
for each element in collection/array
```

JavaScript:

```javascript
for (const user of users) {
    console.log(user);
}
```

### Important difference

Java enhanced `for` index directly provide nahi karta.

Agar index chahiye:

```java
for (int i = 0; i < users.length; i++) {
    ...
}
```

---

# 5.3 `while`

```java
while (condition) {
    // work
}
```

Same fundamental behavior as JavaScript.

---

# 5.4 `do-while`

```java
do {
    // executes at least once
} while (condition);
```

Important:

> `do-while` body **at least once** execute hoti hai.

---

# 5.5 Looping Collections

Modern Java applications mein tum frequently Collections ke saath kaam karoge:

```java
List<User> users = getUsers();

for (User user : users) {
    System.out.println(user.getName());
}
```

JavaScript equivalent:

```javascript
for (const user of users) {
    console.log(user.name);
}
```

---

# 5.6 `forEach`

Java collections mein:

```java
users.forEach(user -> {
    System.out.println(user.getName());
});
```

Ye Java's **lambda expression** use karta hai.

Short form:

```java
users.forEach(user ->
    System.out.println(user.getName())
);
```

JavaScript:

```javascript
users.forEach(user => {
    console.log(user.name);
});
```

Yahan JavaScript se conceptual similarity kaafi strong hai.

But Java ka lambda system **functional interfaces** ke around designed hai.

---

# 5.7 `for` vs `forEach` vs Enhanced `for`

| Approach          | Best use                           |
| ----------------- | ---------------------------------- |
| Traditional `for` | Index/control required             |
| Enhanced `for`    | Simple iteration                   |
| `forEach`         | Functional style / lambda          |
| Stream API        | Transformation/filtering pipelines |

Example:

```java
for (User user : users) {
    if (user.isActive()) {
        System.out.println(user.getName());
    }
}
```

Stream alternative:

```java
users.stream()
     .filter(User::isActive)
     .map(User::getName)
     .forEach(System.out::println);
```

Stream API ek separate major topic hai, isliye abhi usmein deep dive nahi kar rahe.

---

# 5.8 JavaScript vs Java

| JavaScript                    | Java           |
| ----------------------------- | -------------- |
| `for`                         | `for`          |
| `for...of`                    | enhanced `for` |
| `forEach()`                   | `forEach()`    |
| `while`                       | `while`        |
| `do...while`                  | `do...while`   |
| Array dynamic                 | Array fixed    |
| Iteration often loosely typed | Strongly typed |

---

# Common Mistakes

### Mistake 1 — modifying collection during iteration

```java
for (User user : users) {
    users.remove(user);
}
```

Often:

```text
ConcurrentModificationException
```

ka risk hota hai.

Collection modification safely handle karne ke liye appropriate iterator/collection APIs ya filtering approach use karni chahiye.

---

### Mistake 2 — Array vs Collection confusion

```java
numbers.length
```

Array ke liye.

```java
numbers.size()
```

Collection ke liye.

Example:

```java
int[] numbers = {1, 2, 3};

numbers.length;
```

vs

```java
List<Integer> numbers = List.of(1, 2, 3);

numbers.size();
```

**Ye interview mein deliberately test kiya ja sakta hai.**

---

# Interview Perspective

Important questions:

* Enhanced `for` loop kya hai?
* `for` aur enhanced `for` mein difference?
* `forEach()` kya use karta hai?
* `array.length` vs `collection.size()`?
* Collection ko iterate karte waqt modify karne par kya issue aa sakta hai?
* `while` vs `do-while`?

### Must Remember

```text
Array → length
Collection → size()
Enhanced for → for (Type item : collection)
forEach → lambda based
do-while → executes at least once
index required → traditional for
```

---

# 6. `break` and `continue`

Ye JavaScript developers ke liye almost familiar hain, but Java mein **labeled break/continue** ek interesting additional feature hai.

---

# 6.1 `break`

Loop immediately terminate karta hai.

```java
for (int i = 0; i < 100; i++) {
    if (i == 10) {
        break;
    }

    System.out.println(i);
}
```

Output:

```text
0
1
2
...
9
```

`i == 10` par loop terminate.

---

# 6.2 `continue`

Current iteration skip karta hai.

```java
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue;
    }

    System.out.println(i);
}
```

Output:

```text
1
3
5
7
9
```

---

# 6.3 `break` vs `continue`

```text
break
  ↓
entire loop terminate

continue
  ↓
current iteration skip
next iteration
```

---

# 6.4 Nested Loops

```java
for (int i = 0; i < 5; i++) {

    for (int j = 0; j < 5; j++) {

        if (j == 2) {
            break;
        }

        System.out.println(i + ", " + j);
    }
}
```

Important:

> Normal `break` sirf **innermost loop** ko terminate karta hai.

Outer loop continue karega.

---

# 6.5 Labeled `break`

Java ka interesting feature:

```java
outer:
for (int i = 0; i < 5; i++) {

    for (int j = 0; j < 5; j++) {

        if (i == 2 && j == 2) {
            break outer;
        }
    }
}
```

`break outer` directly outer loop se bahar nikalta hai.

Conceptually:

```text
outer loop
   ↓
 inner loop
   ↓
 break outer
   ↓
both loops exit
```

JavaScript developers ke liye ye relatively unfamiliar feature hai.

---

# 6.6 Labeled `continue`

Same concept:

```java
outer:
for (int i = 0; i < 5; i++) {

    for (int j = 0; j < 5; j++) {

        if (j == 2) {
            continue outer;
        }

        System.out.println(i + ", " + j);
    }
}
```

`continue outer` inner loop se outer loop ki next iteration par jump karega.

---

# 6.7 Practical Example

Suppose order processing:

```java
for (Order order : orders) {

    if (order == null) {
        continue;
    }

    if (order.isCancelled()) {
        continue;
    }

    processOrder(order);
}
```

Ye readable use case hai.

`continue` ka use invalid/unwanted records skip karne ke liye reasonable hai.

---

# 6.8 JavaScript vs Java

| Feature            | JavaScript | Java       |
| ------------------ | ---------- | ---------- |
| `break`            | Yes        | Yes        |
| `continue`         | Yes        | Yes        |
| Nested loop break  | Inner loop | Inner loop |
| Labeled `break`    | Supported  | Supported  |
| Labeled `continue` | Supported  | Supported  |

Interesting point:

**JavaScript mein bhi labels exist karte hain**, although production code mein uncommon hain.

---

# Common Mistakes

### `continue` infinite loop create kar sakta hai

Example:

```java
int i = 0;

while (i < 10) {

    if (i == 5) {
        continue;
    }

    i++;
}
```

`i == 5` par `continue` ho raha hai aur `i++` execute nahi ho raha.

Result:

```text
infinite loop
```

Correct:

```java
int i = 0;

while (i < 10) {

    if (i == 5) {
        i++;
        continue;
    }

    i++;
}
```

Ya better logic restructure karo.

---

# Interview Perspective

### Q1. `break` aur `continue` mein difference?

```text
break    → loop terminate
continue → current iteration skip
```

### Q2. Nested loop mein `break` kya karta hai?

Only innermost loop terminate karta hai.

### Q3. Outer loop kaise terminate karoge?

Labeled `break`:

```java
break outer;
```

### Q4. `continue` se infinite loop kab ho sakta hai?

Jab loop condition ko change karne wala update statement `continue` ke baad unreachable ho jaye.

### Must Remember

```text
break → exit loop
continue → skip current iteration
normal break → innermost loop
labeled break → specified outer loop
labeled continue → specified loop's next iteration
continue + missing state update → infinite loop risk
```

---

# 🔥 Experienced JavaScript Developer ke liye Overall Mental Shift

In 6 topics mein sabse important cheez syntax nahi hai. **Mental model** hai.

| JavaScript mindset       | Java mindset                          |
| ------------------------ | ------------------------------------- |
| Dynamic typing           | Static typing                         |
| `number`                 | `byte/short/int/long/float/double`    |
| Everything flexible      | Explicit type rules                   |
| Array dynamic            | Array fixed                           |
| Array for most lists     | `List` for dynamic collections        |
| Function                 | Class/interface method                |
| Dynamic arguments        | Typed parameters / varargs            |
| Implicit coercion common | Conversion controlled                 |
| Object mutation easy     | Reference/value semantics matter      |
| `array.length`           | Array: `length`, Collection: `size()` |
| `for...of`               | Enhanced `for`                        |
| `forEach`                | Lambda + `forEach`                    |
| `break/continue`         | Same + labeled variants               |

---

# 🎯 Interview Revision Sheet

Agar interview se 10 minutes pehle sirf ek section revise karna ho, ye dekho:

### Data Types

```text
8 primitives:
byte, short, int, long,
float, double, char, boolean

String = reference type
int != Integer
char != String
primitive ≠ null
BigDecimal → financial values
```

### Arrays

```text
Array = fixed size
index = 0 based
array.length
last index = length - 1
List/ArrayList = dynamic
generics don't support primitives
int[] default = 0
Integer[] default = null
```

### Methods

```text
Java has methods, not top-level functions
return type explicit
parameters typed
static → class-level
instance → object-level
overloading → different parameter list
Java always pass-by-value
object → copy of reference passed
```

### Type Conversion

```text
widening → implicit
narrowing → explicit cast

String → int
Integer.parseInt()

int → String
String.valueOf()

primitive ↔ wrapper
autoboxing / unboxing

null Integer → int
can cause NPE
```

### Loops

```text
for
while
do-while
enhanced for
forEach

array.length
collection.size()
```

### `break` / `continue`

```text
break → terminate
continue → skip iteration
nested break → inner loop
labeled break → outer loop
```

---

## 🧠 6 Concepts jo tumhe JavaScript se Java mein consciously rewire karne hain

**1. Type-first thinking**

JavaScript:

```javascript
const value = something;
```

Java:

```java
User value = something;
```

Pehle type samajhna important hai.

**2. Array ≠ dynamic list**

Java mein:

```java
int[]        // fixed
List<Integer> // dynamic
```

Is distinction ko strong karo.

**3. Primitive vs Object**

Ye Java ka bahut important foundation hai:

```text
int
Integer
String
User
```

sab same category ke nahi hain.

**4. Pass-by-reference myth**

Java:

> **Always pass-by-value.**

Objects ke case mein reference ki copy pass hoti hai.

**5. Conversion ≠ JavaScript coercion**

Java mein:

```java
(int) 10.99
```

aur

```java
Integer.parseInt("10")
```

jaise explicit mechanisms important hain.

**6. Collections ko jaldi seriously lo**

Real-world Java backend mein arrays se zyada tumhara interaction hoga:

```java
List
Set
Map
Queue
```

etc. ke saath.

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


