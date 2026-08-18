
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

In 6 topics ke baad **next logical jump `String → OOP → Classes/Objects → Constructors → Access Modifiers → Inheritance → Polymorphism → Interfaces → Abstract Classes → Collections → Generics → Exception Handling`** hoga. Yahi point hai jahan JavaScript experience ke bawajood Java ki actual “different way of thinking” strongly start hoti hai.
