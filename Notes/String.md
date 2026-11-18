Strings in Java – Complete Guide (Beginner to Advanced)
Chalo ab Java ka ek bahut important topic detail mein samajhte hain: Strings in Java.

Java backend development mein Strings ka use almost har jagah hota hai:
- User registration aur login.
- API request aur response.
- JSON data processing.
- Database queries.
- JWT tokens.
- Email aur password handling.
- Logging.
- Exception messages.
- File processing.
Aur Java interviews mein bhi String se related kaafi questions pooche jaate hain, especially:
- String immutable kyun hai?
- String Pool kya hota hai?
- == vs .equals() kya difference hai?
- StringBuilder vs StringBuffer.
- String vs char[].
- intern() method kya karta hai?
- Java mein Strings memory mein kaise store hoti hain?
Main assume kar raha hoon ki tum JavaScript mein Strings, Arrays, Objects aur basic programming concepts jaante ho, but Java mein beginner ho.

Isliye hum JavaScript ke saath comparison karte hue har concept ko samjhenge.
Complete topics covered
1
Introduction to Strings

2
String Declaration and Initialization

3
String Immutability

4
String Pool and Heap Memory

5
== vs equals()

6
String Constructors

7
String Methods

8
String Comparison

9
String Searching and Extraction

10
String Concatenation

11
StringBuilder

12
StringBuffer

13
String vs StringBuilder vs StringBuffer

14
String Formatting

15
String Conversion and Parsing

16
String and char[]

17
String Interning

18
String Performance and Memory

19
Regular Expressions

20
Text Blocks

21
Common String Interview Questions

22
Practical Coding Exercises



1. What is a String in Java?
String characters ka sequence hota hai.

For example:
Hello
Java
Rishabh
Hello World
Welcome to Java Programming


Ye sab Strings hain.

JavaScript mein:
let name = "Rishabh";
let language = "Java";
let message = "Welcome to Java";


Java mein:
String name = "Rishabh";
String language = "Java";
String message = "Welcome to Java";


Syntax almost same hai.

Lekin Java mein ek bahut important difference hai:

Java mein String ek class hai, primitive data type nahi.

Java mein primitive data types:
int
long
float
double
char
boolean
byte
short


Aur String ek reference type hai.

Example:
String name = "Rishabh";


Yahan:
- String = Class / reference type.
- name = Reference variable.
- "Rishabh" = String object.
JavaScript mein bhi String primitive type hota hai:
const name = "Rishabh";

console.log(typeof name); // string


Java mein:
String name = "Rishabh";

System.out.println(name.getClass().getName());


Output:
java.lang.String


Important: Java String class java.lang package mein available hai, isliye ise use karne ke liye separately import karne ki zaroorat nahi hoti.

2. How to Create a String in Java?
Java mein String create karne ke multiple ways hain.
Method 1: Using String Literal
Ye sabse common aur recommended way hai.
String name = "Rishabh";

String city = "Ghaziabad";

String language = "Java";


Yahan hum directly double quotes ke andar text likh rahe hain.

Java automatically String object create/manage karta hai aur String Pool ka use bhi karta hai.

String Pool ko hum aage detail mein samjhenge.
Method 2: Using new Keyword
String name = new String("Rishabh");


Yahan hum explicitly new keyword use karke String object create kar rahe hain.

Important difference:
String a = "Java";

String b = new String("Java");


Dono ka content same hai, lekin references different ho sakte hain.

Iska detailed explanation String Pool section mein dekhenge.
Method 3: Using Character Array
char[] characters = {'J', 'a', 'v', 'a'};

String language = new String(characters);

System.out.println(language);


Output:
Java


Method 4: Using String Constructor with Bytes
byte[] data = {72, 101, 108, 108, 111};

String message = new String(data);

System.out.println(message);


Output:
Hello


Yahan bytes ko default charset ke according String mein convert kiya ja raha hai.

Production code mein charset explicitly specify karna better hai:
import java.nio.charset.StandardCharsets;

byte[] data = {72, 101, 108, 108, 111};

String message = new String(data, StandardCharsets.UTF_8);


Quick Comparison
Method	Example	Recommended
String Literal	"Java"	Yes
new String()	new String("Java")	Usually no
char[]	new String(chars)	When needed
byte[]	new String(bytes, UTF_8)	When decoding bytes


3. String Immutability in Java
Very important interview topic
Ab String ka sabse important concept samajhte hain: Immutability.

Pehle immutability ka meaning samjho.

Immutability ka meaning hai:

Once an object is created, uske internal content ko change nahi kar sakte.

Java mein String immutable hoti hai.

Example:
public class Main {

    public static void main(String[] args) {

        String name = "Rishabh";

        name.concat(" Kumar");

        System.out.println(name);
    }
}


Output:
Rishabh


Tum expect kar rahe hoge:
Rishabh Kumar


Lekin aisa nahi hua.

Why?

Because Java String immutable hai.

Jab tum:
name.concat(" Kumar");


execute karte ho, toh existing String modify nahi hoti. Ek new String result create hota hai, lekin tumne us result ko kisi variable mein store nahi kiya.

Correct approach:
String name = "Rishabh";

name = name.concat(" Kumar");

System.out.println(name);


Output:
Rishabh Kumar


Let's understand with a visual example
Step 1: Initial String
"Rishabh"
Original String object

Step 2: Execute concat(" Kumar")

"Rishabh"
Unchanged


"Rishabh Kumar"
New String



The concat operation returns a new String. It does not modify the original object.
Ek aur example:
String language = "Java";

language.toUpperCase();

System.out.println(language);


Output:
Java


Correct:
language = language.toUpperCase();

System.out.println(language);


Output:
JAVA


JavaScript Comparison
JavaScript mein bhi String primitives immutable hote hain.
let name = "Rishabh";

name.toUpperCase();

console.log(name); // Rishabh


Same behavior!

However, Java mein String ek class/object hai, jabki JavaScript mein String primitive hai (although JavaScript mein String wrapper object bhi hota hai).
Important: Immutability ka matlab variable final hona nahi hai
Ye dono alag concepts hain.

Example:
String name = "Rishabh";

name = "Rahul";


Ye perfectly valid hai.

Kyunki tum variable ko new String object se reassign kar rahe ho.

Lekin:
final String name = "Rishabh";

name = "Rahul"; // Compilation error


Yahan final ki wajah se reassignment allowed nahi hai.

Difference:
Concept	Meaning
Immutable	Object ka content change nahi hota
final	Variable ko reassign nahi kar sakte

Interview point: String immutable hai, lekin String variable final hona compulsory nahi hai.

4. Why is String Immutable in Java?
Interviews mein frequently poocha jata hai:

Why did Java make String immutable?

Iske multiple important reasons hain.
1. Security
Suppose tum database connection create kar rahe ho:
String connection = "jdbc:mysql://localhost:3306/mydb";


Ya file path:
String filePath = "/home/user/important-file.txt";


Agar String mutable hoti, toh same object ko modify karke sensitive values change karna possible hota.

Immutability unexpected modifications ka risk reduce karti hai.
2. String Pool Optimization
Java String Pool mein multiple variables same String object share kar sakte hain.

Example:
String a = "Java";

String b = "Java";


Dono same pooled object ko refer kar sakte hain.

Agar String mutable hoti, toh a ke through object change karne par b ka content bhi change ho jata.

Immutability is sharing ko safe banati hai.
3. Thread Safety
Multiple threads same String object ko safely read kar sakte hain because its content cannot change.

Isliye synchronization ki zaroorat nahi hoti for reading the String's content.
4. HashCode Caching
Strings ko HashMap aur HashSet mein keys ke roop mein frequently use kiya jata hai.

String immutable hone ki wajah se uska hash code change nahi hota.

Java String implementation hash code cache kar sakti hai, jisse repeated hash calculations avoid hoti hain.
Interview Answer
String immutable hone ke major reasons hain:
1. Security.
2. String Pool optimization.
3. Thread safety.
4. HashCode caching and reliable hash-based collections.

5. String Pool in Java
Extremely important
Ab String Pool samajhte hain.

String Pool ko String Constant Pool (SCP) bhi kaha jata hai.

Ye JVM ke heap memory area mein maintained ek special pool of Strings hai.

Iska main purpose hai String objects ko reuse karna aur unnecessary duplicate objects reduce karna.
Without String Pool
Imagine karo tumhare application mein 1000 places par same String create ho rahi hai:
String a = "Java";
String b = "Java";
String c = "Java";
String d = "Java";


Agar har baar separate String object create hota, toh memory unnecessarily consume hoti.
With String Pool
Java same String literal ko reuse kar sakta hai.
String a = "Java";

String b = "Java";

String c = "Java";


Teeno references same pooled String object ko point kar sakte hain.
String Pool – Memory Visualization
a

Reference

b

Reference

c

Reference



String Pool
"Java"
One shared String object


Example 1: String Literals
public class Main {

    public static void main(String[] args) {

        String a = "Java";

        String b = "Java";

        System.out.println(a == b);

        System.out.println(a.equals(b));
    }
}


Output:
true
true


Why?
- a == b true because both references point to the same pooled object.
- a.equals(b) true because both contain the same text.
Example 2: Using new Keyword
public class Main {

    public static void main(String[] args) {

        String a = "Java";

        String b = new String("Java");

        System.out.println(a == b);

        System.out.println(a.equals(b));
    }
}


Output:
false
true


Why?

Let's understand:
String a = "Java";


String Pool mein "Java" object reuse/create hota hai.
String b = new String("Java");


new keyword explicitly heap mein ek separate String object create karta hai.

Even if "Java" already exists in the String Pool, new String() creates a new String object.
String Literal vs new String()
String Pool
"Java"

a points here


Heap
"Java"

b points here



Same content, different String objects.
Result:
a == b       // false
a.equals(b)  // true


Remember: == references compare karta hai, .equals() content compare karta hai.

6. Understanding == vs .equals() in Java Strings
Ye concept itna important hai ki isko separately samajhna zaroori hai.

Java mein String compare karne ke 2 common ways hain:
1. == operator
2. .equals() method
A. Using ==
== operator check karta hai ki dono reference variables same object ko point kar rahe hain ya nahi.
String a = new String("Hello");

String b = new String("Hello");

System.out.println(a == b);


Output:
false


Kyunki dono separate objects hain.
B. Using .equals()
.equals() actual String content compare karta hai.
String a = new String("Hello");

String b = new String("Hello");

System.out.println(a.equals(b));


Output:
true


Kyunki dono ka content same hai.
C. Using equalsIgnoreCase()
Agar tum case-sensitive comparison nahi karna chahte, toh:
String a = "JAVA";

String b = "java";

System.out.println(a.equals(b));

System.out.println(a.equalsIgnoreCase(b));


Output:
false
true


Complete Comparison
Operator / Method	What it checks
'==	Reference equality
equals()	Content equality
equalsIgnoreCase()	Content equality ignoring case
compareTo()	Lexicographical comparison
compareToIgnoreCase()	Lexicographical comparison ignoring case

Practical Example: User Login
Suppose tum user ka username verify kar rahe ho.

Incorrect:
String enteredUsername = new String("admin");

if (enteredUsername == "admin") {
    System.out.println("Login successful");
}


Ye false ho sakta hai.

Correct:
String enteredUsername = new String("admin");

if (enteredUsername.equals("admin")) {
    System.out.println("Login successful");
}


Production code mein null-safe comparison bhi useful hai:
String expectedUsername = "admin";

if (expectedUsername.equals(enteredUsername)) {
    System.out.println("Login successful");
}


Agar enteredUsername null bhi ho, tab bhi is comparison mein NullPointerException nahi aayega.

7. String Pool and intern() Method
Ab String Pool ka ek advanced concept samajhte hain: intern().

intern() method ka use String Pool se canonical String reference obtain karne ke liye hota hai.

Example:
public class Main {

    public static void main(String[] args) {

        String a = new String("Java");

        String b = a.intern();

        String c = "Java";

        System.out.println(a == b);

        System.out.println(b == c);
    }
}


Output:
false
true


Samjho:
String a = new String("Java");


Separate String object create hua.
String b = a.intern();


String Pool mein "Java" ka canonical reference return hua.
String c = "Java";


Ye same pooled object ko refer karta hai.

Isliye:
b == c // true


When should we use intern()?
Mostly specialized situations mein, jab large amounts of repeated Strings ko canonicalize karna ho.

Normal backend development mein har String par intern() use karna recommended nahi hai. Iska indiscriminate use memory pressure create kar sakta hai.

8. Important String Methods in Java
Ab hum Java String class ke most useful methods cover karenge.

Java backend development mein in methods ka frequent use hoga.

Let's create one String:
String message = "Hello Java Programming";


1. length()
String mein total UTF-16 code units ki count return karta hai.
String name = "Rishabh";

System.out.println(name.length());


Output:
7


JavaScript:
name.length;


Dono mein .length use hota hai.

Note: Normal English text mein character count aur length generally same hoti hai. Emoji jaise supplementary Unicode characters ke case mein difference ho sakta hai.
2. charAt()
Specific index par character return karta hai.
String name = "Rishabh";

System.out.println(name.charAt(0));

System.out.println(name.charAt(3));


Output:
R
h


JavaScript:
name.charAt(0);


Same concept.
3. substring()
String ka ek particular part extract karta hai.
String message = "Hello Java Programming";

System.out.println(message.substring(6));

System.out.println(message.substring(6, 10));


Output:
Java Programming
Java


Syntax:
substring(startIndex)

substring(startIndex, endIndex)


Important:
- Start index inclusive hota hai.
- End index exclusive hota hai.
JavaScript comparison:
message.substring(6, 10);


Syntax almost same hai.
4. contains()
Check karta hai ki String mein particular text present hai ya nahi.
String message = "Welcome to Java";

System.out.println(message.contains("Java"));

System.out.println(message.contains("Python"));


Output:
true
false


JavaScript:
message.includes("Java");


5. startsWith()
Check karta hai ki String kisi particular text se start ho rahi hai ya nahi.
String url = "https://example.com";

System.out.println(url.startsWith("https"));


Output:
true


JavaScript:
url.startsWith("https");


6. endsWith()
Check karta hai ki String kisi particular text ke saath end ho rahi hai ya nahi.
String fileName = "document.pdf";

System.out.println(fileName.endsWith(".pdf"));


Output:
true


JavaScript:
fileName.endsWith(".pdf");


7. toUpperCase()
String ko uppercase mein convert karta hai.
String name = "java programming";

System.out.println(name.toUpperCase());


Output:
JAVA PROGRAMMING


8. toLowerCase()
String name = "JAVA PROGRAMMING";

System.out.println(name.toLowerCase());


Output:
java programming


9. trim()
String ke beginning aur ending se whitespace remove karta hai.
String name = "   Rishabh   ";

System.out.println(name.trim());


Output:
Rishabh


Important: trim() traditional whitespace characters remove karta hai. Modern Java mein Unicode-aware whitespace handling ke liye strip() bhi available hai.
10. strip()
String name = "   Rishabh   ";

System.out.println(name.strip());


Output:
Rishabh


strip() Java 11 se available hai.

Related methods:
stripLeading();
stripTrailing();


11. replace()
String mein kisi character ya text ko replace karta hai.
String message = "I love Java";

System.out.println(message.replace("Java", "Spring Boot"));


Output:
I love Spring Boot


Character replacement:
String number = "123-456-789";

System.out.println(number.replace('-', ' '));


Output:
123 456 789


12. replaceAll()
Regular expression ke basis par replacement karta hai.
String phone = "987-654-3210";

String result = phone.replaceAll("[^0-9]", "");

System.out.println(result);


Output:
9876543210


Yahan [^0-9] ka meaning hai: digits ke alawa koi bhi character.
13. replaceFirst()
Regex ke matching first occurrence ko replace karta hai.
String message = "Java is good. Java is powerful.";

System.out.println(message.replaceFirst("Java", "Spring"));


Output:
Spring is good. Java is powerful.


14. indexOf()
Particular character ya substring ka first index return karta hai.
String message = "Hello Java";

System.out.println(message.indexOf("Java"));

System.out.println(message.indexOf('o'));


Output:
6
4


Agar element nahi milta toh -1 return hota hai.
15. lastIndexOf()
Last occurrence ka index return karta hai.
String message = "Java is powerful. Java is popular.";

System.out.println(message.lastIndexOf("Java"));


Output:
19


16. isEmpty()
Check karta hai ki String ki length zero hai ya nahi.
String name = "";

System.out.println(name.isEmpty());


Output:
true


Important:
String name = "   ";

System.out.println(name.isEmpty());


Output:
false


Spaces present hain, isliye empty nahi hai.
17. isBlank()
Check karta hai ki String empty hai ya sirf whitespace contain karti hai.
String name = "   ";

System.out.println(name.isBlank());


Output:
true


isBlank() Java 11 se available hai.
18. concat()
Do Strings ko join karta hai.
String firstName = "Rishabh";

String lastName = "Kumar";

String fullName = firstName.concat(" ").concat(lastName);

System.out.println(fullName);


Output:
Rishabh Kumar


19. repeat()
String ko multiple times repeat karta hai.
String name = "Java ";

System.out.println(name.repeat(3));


Output:
Java Java Java


repeat() Java 11 se available hai.
20. split()
String ko multiple parts mein divide karta hai.
String employees = "Rahul,Amit,Rishabh,Priya";

String[] names = employees.split(",");

for (String name : names) {
    System.out.println(name);
}


Output:
Rahul
Amit
Rishabh
Priya


JavaScript comparison:
const employees = "Rahul,Amit,Rishabh,Priya";

const names = employees.split(",");


Same concept.

Important: Java ka split() regex accept karta hai. Agar delimiter special regex character hai, toh escape karna padega.

Example:
String data = "Java|Spring|SQL";

String[] values = data.split("\\|");


21. toCharArray()
String ko character array mein convert karta hai.
String name = "Java";

char[] characters = name.toCharArray();

for (char character : characters) {
    System.out.println(character);
}


Output:
J
a
v
a


22. valueOf()
Different data types ko String mein convert karta hai.
int age = 30;

double salary = 75000.50;

boolean active = true;

String a = String.valueOf(age);

String b = String.valueOf(salary);

String c = String.valueOf(active);

System.out.println(a);
System.out.println(b);
System.out.println(c);


Output:
30
75000.5
true


JavaScript comparison:
String(30);
String(75000.50);
String(true);


Quick Reference Table
Method	Purpose
length()	String length
charAt()	Character at index
substring()	Extract substring
contains()	Check text
startsWith()	Check prefix
endsWith()	Check suffix
toUpperCase()	Uppercase
toLowerCase()	Lowercase
trim()	Remove edge whitespace
strip()	Unicode-aware whitespace removal
replace()	Replace literal text/character
replaceAll()	Regex replacement
indexOf()	First occurrence
lastIndexOf()	Last occurrence
isEmpty()	Check empty
isBlank()	Check blank
concat()	Join Strings
repeat()	Repeat String
split()	Divide into array
toCharArray()	Convert to char array
valueOf()	Convert value to String


9. String Comparison Using compareTo()
Ab ek aur important method samajhte hain.

compareTo() Strings ko lexicographical order mein compare karta hai.

Example:
String a = "Apple";

String b = "Banana";

System.out.println(a.compareTo(b));


Output:
Negative value


Generally:
- Negative result: First String lexicographically smaller hai.
- Zero: Dono Strings equal hain.
- Positive result: First String lexicographically greater hai.
Exact result ko fixed -1, 0, 1 assume mat karna. Method difference of character values return kar sakta hai.

Example:
String a = "Java";

String b = "Java";

System.out.println(a.compareTo(b));


Output:
0


Case-insensitive comparison:
String a = "JAVA";

String b = "java";

System.out.println(a.compareToIgnoreCase(b));


Output:
0


JavaScript comparison:
"Apple".localeCompare("Banana");


Concept similar hai, although JavaScript localeCompare() locale-aware comparison provide karta hai.

10. String Concatenation in Java
Concatenation ka meaning hai multiple Strings ko combine karna.

Java mein multiple ways available hain.
Method 1: Using + Operator
String firstName = "Rishabh";

String lastName = "Kumar";

String fullName = firstName + " " + lastName;

System.out.println(fullName);


Output:
Rishabh Kumar


JavaScript:
const fullName = firstName + " " + lastName;


Method 2: Using concat()
String firstName = "Rishabh";

String lastName = "Kumar";

String fullName = firstName.concat(" ").concat(lastName);


Method 3: Using String.join()
String result = String.join(" ", "Java", "Spring", "Boot");

System.out.println(result);


Output:
Java Spring Boot


Comma-separated example:
String result = String.join(",", "Rahul", "Amit", "Priya");


Output:
Rahul,Amit,Priya


Method 4: Using StringBuilder
StringBuilder builder = new StringBuilder();

builder.append("Java");

builder.append(" ");

builder.append("Spring");

builder.append(" Boot");

String result = builder.toString();

System.out.println(result);


Output:
Java Spring Boot


StringBuilder ko hum next section mein detail se samjhenge.
Important: String Concatenation Internally
Suppose:
String a = "Hello";

String b = "Java";

String result = a + " " + b;


Java compiler concatenation ko optimize kar sakta hai. Runtime concatenation ke case mein implementation compiler/JDK ke according differ kar sakti hai.

Repeated concatenation loops mein expensive ho sakti hai because Strings immutable hoti hain.

Example:
String result = "";

for (int i = 0; i < 1000; i++) {
    result = result + i;
}


Har iteration mein new String result create ho sakta hai.

Better:
StringBuilder result = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    result.append(i);
}

String finalResult = result.toString();


Ye generally repeated concatenation ke comparison mein more efficient approach hai.

11. StringBuilder in Java
Very important for backend development
Ab hum StringBuilder samjhenge.

Pehle ek question:

Agar Java String immutable hai, toh hum repeatedly String mein new text kaise add karenge efficiently?

Is problem ko solve karne ke liye Java mein StringBuilder class available hai.

StringBuilder ek mutable sequence of characters hai.

Meaning: Iske existing content ko modify kar sakte ho, bina har operation par new String object create kiye.
String vs StringBuilder
String example:
String name = "Java";

name = name + " Programming";

name = name + " Language";


Har concatenation ek new String result create karta hai.

StringBuilder example:
StringBuilder name = new StringBuilder("Java");

name.append(" Programming");

name.append(" Language");

System.out.println(name);


Output:
Java Programming Language


Yahan existing StringBuilder object modify ho raha hai.
StringBuilder ka object kaise create karein?
Method 1:
StringBuilder builder = new StringBuilder();


Empty StringBuilder.

Method 2:
StringBuilder builder = new StringBuilder("Java");


Initial content ke saath.

Method 3:
StringBuilder builder = new StringBuilder(100);


Initial capacity 100 characters ke liye reserve karne ki request.

Note: Capacity ka matlab actual String length nahi hai. Capacity internal storage ki available space hai.
12. Important StringBuilder Methods
StringBuilder ke methods samajhna important hai.
1. append()
Existing content ke end mein new content add karta hai.
StringBuilder builder = new StringBuilder("Hello");

builder.append(" Java");

builder.append(" Programming");

System.out.println(builder);


Output:
Hello Java Programming


2. insert()
Specific index par content insert karta hai.
StringBuilder builder = new StringBuilder("Hello");

builder.insert(5, " Java");

System.out.println(builder);


Output:
Hello Java


3. replace()
Specific range ka content replace karta hai.
StringBuilder builder = new StringBuilder("Hello Java");

builder.replace(6, 10, "World");

System.out.println(builder);


Output:
Hello World


Remember: End index exclusive hota hai.
4. delete()
Specific range delete karta hai.
StringBuilder builder = new StringBuilder("Hello Java");

builder.delete(5, 10);

System.out.println(builder);


Output:
Hello


5. deleteCharAt()
Specific index ka character delete karta hai.
StringBuilder builder = new StringBuilder("Hello");

builder.deleteCharAt(1);

System.out.println(builder);


Output:
Hllo


6. reverse()
StringBuilder ka content reverse karta hai.
StringBuilder builder = new StringBuilder("Java");

builder.reverse();

System.out.println(builder);


Output:
avaJ


Ye Java String ka direct method nahi hai, lekin StringBuilder mein available hai.
7. capacity()
Internal buffer ki current capacity return karta hai.
StringBuilder builder = new StringBuilder();

System.out.println(builder.capacity());


Output:
16


Default initial capacity 16 hoti hai.

Example:
StringBuilder builder = new StringBuilder(50);

System.out.println(builder.capacity());


Output:
50


8. length()
Currently kitne characters hain, wo return karta hai.
StringBuilder builder = new StringBuilder("Java");

System.out.println(builder.length());


Output:
4


Difference:
- length() = Current content length.
- capacity() = Available internal buffer capacity.
9. setLength()
Builder ki current length set karta hai.
StringBuilder builder = new StringBuilder("Hello Java");

builder.setLength(5);

System.out.println(builder);


Output:
Hello


Agar current length se larger value set karoge, toh additional positions null characters (\u0000) se fill hongi.
10. charAt()
Specific index ka character return karta hai.
StringBuilder builder = new StringBuilder("Java");

System.out.println(builder.charAt(0));


Output:
J


11. setCharAt()
Specific index par character update karta hai.
StringBuilder builder = new StringBuilder("Java");

builder.setCharAt(0, 'K');

System.out.println(builder);


Output:
Kava


12. toString()
StringBuilder ko normal String mein convert karta hai.
StringBuilder builder = new StringBuilder("Hello Java");

String result = builder.toString();

System.out.println(result);


Output:
Hello Java


StringBuilder Methods – Quick Reference
Method	Purpose
append()	Add at end
insert()	Insert at index
replace()	Replace a range
delete()	Delete a range
deleteCharAt()	Delete one character
reverse()	Reverse content
capacity()	Get capacity
length()	Get current length
setLength()	Change current length
charAt()	Get character
setCharAt()	Update character
toString()	Convert to String


13. StringBuffer in Java
Ab StringBuilder ke similar ek aur class hai: StringBuffer.

StringBuffer bhi mutable character sequence provide karta hai.

Meaning: Ismein bhi content modify kar sakte ho.

Example:
StringBuffer buffer = new StringBuffer("Java");

buffer.append(" Programming");

System.out.println(buffer);


Output:
Java Programming


Tum soch rahe hoge ki StringBuilder aur StringBuffer same hain, toh dono kyun exist karte hain?

Inka main difference hai Thread Safety.
What is Thread Safety?
Suppose tumhare application mein multiple threads hain.

Multiple threads simultaneously same object ko access aur modify karna chahte hain.

Aise scenario mein shared data ko safely manage karna important hota hai.

StringBuffer ke methods synchronized hote hain, isliye individual operations par thread-safe behavior provide karte hain.

StringBuilder synchronized nahi hai.
StringBuilder Example
StringBuilder builder = new StringBuilder("Java");

builder.append(" Spring");


Fast in typical single-threaded usage, but no built-in synchronization.
StringBuffer Example
StringBuffer buffer = new StringBuffer("Java");

buffer.append(" Spring");


Methods synchronized hote hain, so shared access scenarios mein additional protection milti hai.
Important: Iska matlab StringBuffer completely thread-safe application logic guarantee nahi karta
For example, multiple method calls ko ek atomic operation banana ho, toh additional synchronization ki zaroorat ho sakti hai.

Also, synchronization ki wajah se StringBuffer generally StringBuilder se slower ho sakta hai.

14. String vs StringBuilder vs StringBuffer
Frequently asked interview question
Ye teen classes ka difference properly samajh lo.
Feature	String	StringBuilder	StringBuffer
Mutable	No	Yes	Yes
Thread-safe	Immutable content	No	Synchronized methods
Performance for repeated modification	Less efficient	Generally fastest	Synchronization overhead
String Pool	Yes	No	No
Introduced	Java 1.0	Java 5	Java 1.0
Use case	Fixed text	Repeated modifications	Shared mutable text

When should you use which?
Use String when:
- Normal text store karna ho.
- Username, email, API response.
- Configuration values.
- Text frequently modify nahi ho raha.
Use StringBuilder when:
- Loops mein String build karni ho.
- Dynamic SQL/query generation.
- Large text generate karna ho.
- JSON/text construction.
- Single-threaded local text manipulation.
Use StringBuffer when:
- Shared mutable character sequence par synchronized operations required hon.
- Legacy code mein StringBuffer already use ho raha ho.
My recommendation: Modern Java backend development mein generally String aur StringBuilder most frequently use honge. StringBuffer ka concept bhi samajhna zaroori hai.

15. String Formatting in Java
Java mein formatted Strings create karne ke multiple ways hain.

Suppose tumhe employee details display karni hain.
String name = "Rishabh";

int age = 30;

double salary = 85000.50;


Method 1: Using printf()
System.out.printf(
    "Name: %s, Age: %d, Salary: %.2f",
    name,
    age,
    salary
);


Output:
Name: Rishabh, Age: 30, Salary: 85000.50


Common format specifiers:
Specifier	Meaning
%s	String
%d	Integer
%f	Floating point
%.2f	2 decimal places
%c	Character
%b	Boolean
%n	Platform-specific newline

Method 2: Using String.format()
String result = String.format(
    "Name: %s, Age: %d",
    name,
    age
);

System.out.println(result);


String.format() formatted String return karta hai.
Method 3: Using Formatted() – Java 15+
String result = "Name: %s, Age: %d".formatted(name, age);

System.out.println(result);


Ye String.format() ka convenient alternative hai.
Method 4: Using Text Blocks
Java mein multiline Strings ko readable format mein likhne ke liye text blocks available hain.

Java 15 se standard feature hai.
String message = """
        Hello Rishabh,
        Welcome to Java.
        Happy Learning!
        """;

System.out.println(message);


Output:
Hello Rishabh,
Welcome to Java.
Happy Learning!


Ye JSON, SQL queries aur multiline templates ke liye useful hai.

16. String Conversion and Parsing
Backend development mein frequently String ko number mein aur number ko String mein convert karna hota hai.

Example:

API se tumhe age "30" ke form mein milti hai, lekin tumhe integer value chahiye.
A. String to Integer
String age = "30";

int result = Integer.parseInt(age);

System.out.println(result + 10);


Output:
40


JavaScript:
const age = "30";

const result = parseInt(age, 10);


B. String to Double
String price = "150.75";

double result = Double.parseDouble(price);

System.out.println(result);


Output:
150.75


C. String to Long
String value = "9876543210";

long result = Long.parseLong(value);


D. Integer to String
int age = 30;

String result = String.valueOf(age);


Alternative:
String result = Integer.toString(age);


E. Double to String
double salary = 85000.50;

String result = String.valueOf(salary);


F. Parsing Errors
Important:
String age = "thirty";

int result = Integer.parseInt(age);


Ye exception throw karega:
NumberFormatException


Production code mein invalid user input handle karna zaroori hai.

Example:
public class Main {

    public static void main(String[] args) {

        String age = "30";

        try {

            int result = Integer.parseInt(age);

            System.out.println(result);

        } catch (NumberFormatException e) {

            System.out.println("Invalid age provided");

        }
    }
}


Conversion Quick Reference
Conversion	Java
String → int	Integer.parseInt()
String → double	Double.parseDouble()
String → long	Long.parseLong()
String → boolean	Boolean.parseBoolean()
int → String	String.valueOf()
double → String	String.valueOf()
Object → String	String.valueOf()

Note: Boolean.parseBoolean("true") true deta hai; other values, including "yes", false return karti hain. Ye strict validation method nahi hai.

17. String vs char[] in Java
Ye bhi important concept hai.

Java mein String aur character array dono text represent kar sakte hain, but dono different hain.
String Example
String name = "Rishabh";


String immutable hai.
char[] Example
char[] name = {'R', 'i', 's', 'h', 'a', 'b', 'h'};


Character array mutable hai.

Example:
char[] name = {'J', 'a', 'v', 'a'};

name[0] = 'K';

System.out.println(name);


Output:
Kava


String mein directly character update nahi kar sakte:
String name = "Java";

name[0] = 'K'; // Compilation error


Comparison
Feature	String	char[]
Mutable	No	Yes
String Pool	Yes	No
Built-in methods	Many	Limited
Content modification	New String	Direct modification
Security-sensitive temporary data	Less suitable	Can be cleared manually

Why is char[] sometimes preferred for passwords?
Example:
char[] password = {'s', 'e', 'c', 'r', 'e', 't'};

// Use password...

Arrays.fill(password, '\0');


Character array ke elements ko manually overwrite kar sakte hain.

String ko immutable hone ki wajah se manually clear nahi kar sakte.

Lekin dhyan rahe: char[] use karna automatically perfect security guarantee nahi karta. Copies, logs, memory dumps aur garbage collection still matter karte hain.

18. String Immutability vs StringBuilder Mutability – Practical Comparison
Chalo ek real example lete hain.

Suppose tumhe 1000 employee names combine karke ek single String banani hai.
Using String
public class Main {

    public static void main(String[] args) {

        String result = "";

        for (int i = 1; i <= 1000; i++) {

            result = result + "Employee" + i + " ";

        }

        System.out.println(result);
    }
}


Ye repeatedly new String objects create kar sakta hai.
Using StringBuilder
public class Main {

    public static void main(String[] args) {

        StringBuilder result = new StringBuilder();

        for (int i = 1; i <= 1000; i++) {

            result.append("Employee");
            result.append(i);
            result.append(" ");

        }

        System.out.println(result.toString());
    }
}


Ye generally more efficient approach hai.

Interview question: StringBuilder String se faster kyun hota hai repeated modifications mein?

Answer: String immutable hai, isliye concatenation new String results create kar sakti hai. StringBuilder mutable hai aur internally resizable character storage use karta hai, isliye repeated append operations generally less allocation ke saath perform hote hain.

19. Regular Expressions (Regex) with Java Strings
Regex ka use Strings ke andar specific patterns search, validate aur replace karne ke liye hota hai.

Backend development mein bahut useful hai:
- Email validation.
- Mobile number validation.
- Password validation.
- Extracting numbers from text.
- Removing unwanted characters.
- Validating API input.
Java mein regex ke liye Pattern aur Matcher classes available hain.

Package:
import java.util.regex.Pattern;
import java.util.regex.Matcher;


A. Basic Regex Using matches()
Suppose tumhe check karna hai ki String sirf digits contain karti hai ya nahi.
String number = "9876543210";

boolean result = number.matches("[0-9]+");

System.out.println(result);


Output:
true


Explanation:
- [0-9] = Any digit from 0 to 9.
- + = One or more occurrences.
B. Email Pattern Example
String email = "test@gmail.com";

boolean valid = email.matches(
    "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$"
);

System.out.println(valid);


Output:
true


Note: Ye basic pattern hai, complete email validation nahi. Production applications mein email validation ke liye proper validation libraries aur verification flow use karna better hai.
C. Mobile Number Validation
India ke 10-digit mobile number ka basic pattern:
String mobile = "9876543210";

boolean valid = mobile.matches("[6-9][0-9]{9}");

System.out.println(valid);


Output:
true


D. Password Validation
Suppose password mein:
- Minimum 8 characters.
- At least one uppercase.
- At least one lowercase.
- At least one digit.
String password = "Java1234";

boolean valid = password.matches(
    "^(?=.*[A-Z])(?=.*[a-z])(?=.*\\d).{8,}$"
);

System.out.println(valid);


Output:
true


Yahan:
Regex	Meaning
(?=.*[A-Z])	At least one uppercase
(?=.*[a-z])	At least one lowercase
(?=.*\\d)	At least one digit
.{8,}	Minimum 8 characters

Java mein regex ke backslashes ko escape karna padta hai. Isliye \d ko "\\d" likhte hain.
E. Using Pattern and Matcher
import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class Main {

    public static void main(String[] args) {

        String text = "My age is 30 and my salary is 85000";

        Pattern pattern = Pattern.compile("\\d+");

        Matcher matcher = pattern.matcher(text);

        while (matcher.find()) {

            System.out.println(matcher.group());

        }
    }
}


Output:
30
85000


Yahan:
- Pattern.compile() regex pattern compile karta hai.
- pattern.matcher() input String ke against Matcher create karta hai.
- matcher.find() next matching sequence search karta hai.
- matcher.group() matched text return karta hai.
Important: Agar same regex ko repeatedly use karna hai, toh compiled Pattern ko reuse karna efficient ho sakta hai.

20. Unicode, UTF-16 and Java String
Ye thoda advanced topic hai, lekin Java Strings ko deeply samajhne ke liye important hai.

Java String Unicode text represent karti hai.

Unicode ek standard hai jo different languages ke characters aur symbols ko represent karta hai.

For example:
String english = "Hello";

String hindi = "नमस्ते";

String emoji = "😊";


Java in sabko String mein represent kar sakta hai.
What is UTF-16?
Java String internally UTF-16 code units ke form mein text represent karti hai.

Isliye:
String name = "Java";

System.out.println(name.length());


Output:
4


Lekin emoji ke case mein:
String emoji = "😊";

System.out.println(emoji.length());


Output:
2


Tum sochoge emoji toh ek hai, phir length 2 kyun?

Because ye emoji UTF-16 mein surrogate pair use karta hai, yaani 2 code units.
Code Points
Actual Unicode code points count karne ke liye:
String emoji = "😊";

System.out.println(emoji.codePointCount(0, emoji.length()));


Output:
1


More examples
String text = "Hello 😊";

System.out.println(text.length());

System.out.println(
    text.codePointCount(0, text.length())
);


Output:
8
7


Yahan:
- UTF-16 code units = 8.
- Unicode code points = 7.
JavaScript mein bhi .length UTF-16 code units count karta hai.
"😊".length; // 2

[..."😊"].length; // 1


Interview takeaway: Java String ka length() method Unicode code points ya user-perceived characters count nahi karta. Ye UTF-16 code units count karta hai.

21. Escape Characters in Java Strings
Java mein kuch special characters directly normal way mein use nahi kar sakte.

Inke liye escape sequences use hote hain.

Example:
New Line – \n
String message = "Hello\nJava";

System.out.println(message);


Output:
Hello
Java


Tab – \t
String message = "Hello\tJava";

System.out.println(message);


Output:
Hello    Java


Double Quote – \"
String message = "Welcome to \"Java\"";

System.out.println(message);


Output:
Welcome to "Java"


Backslash – \\
String path = "C:\\Users\\Rishabh\\Documents";

System.out.println(path);


Output:
C:\Users\Rishabh\Documents


Common Escape Sequences
Escape	Meaning
\n	New line
\t	Tab
\r	Carriage return
\\	Backslash
\"	Double quote
\'	Single quote
\b	Backspace
\f	Form feed


22. Null vs Empty String vs Blank String
Backend development mein ye distinction bahut important hai.

Teen examples dekho:
String a = null;

String b = "";

String c = "   ";


Ye teeno different situations represent karte hain.
Value	Meaning	isEmpty()	isBlank()
null	No String object	N/A	N/A
""	Empty String	true	true
"   "	Whitespace only	false	true
"Java"	Actual text	false	false

Important: null par directly String method call karoge toh NullPointerException aayega.

Example:
String name = null;

System.out.println(name.length());


Error:
NullPointerException


How to Handle Null?
String name = null;

if (name != null && !name.isBlank()) {

    System.out.println("Valid name");

} else {

    System.out.println("Name is missing");

}


Java 11+ mein:
if (name == null || name.isBlank()) {
    System.out.println("Name is required");
}


JavaScript comparison:
if (!name || !name.trim()) {
    console.log("Name is required");
}


Concept similar hai, although Java mein null handling explicitly karna important hai.

23. String Internals – How Java Stores Strings
Ab thoda JVM internals samajhte hain.

Jab tum likhte ho:
String name = "Java";


Simplified explanation:
1. JVM String literal ko String Pool mein check karta hai.
2. Agar same literal already present hai, toh existing object reuse hota hai.
3. Agar nahi hai, toh pooled String object create hota hai.
4. name reference us object ko point karta hai.
What is Heap Memory?
Heap JVM memory ka area hai jahan objects allocate hote hain.

Java 7 se String Pool heap memory mein located hai. Older Java versions mein iska placement different tha.
What is Stack Memory?
Method execution ke dauran local variables aur references ke liye JVM stack frames use karta hai.

Simplified example:
public class Main {

    public static void main(String[] args) {

        String name = "Java";

        String city = new String("Delhi");

    }
}


Conceptually:
Stack – main() frame
name
Reference

city
Reference



Heap Memory
String Pool
"Java"

Separate String object
"Delhi"



Ye simplified conceptual representation hai. Actual JVM implementation mein compiler optimizations aur object allocation details vary kar sakti hain.
What about Compact Strings?
Java 9 se OpenJDK ki String implementation mein Compact Strings optimization introduce hui.

Java internally Latin-1 compatible text ko generally 1 byte per character storage representation mein aur other UTF-16 content ko 2 bytes per code unit representation mein store kar sakta hai.

Ye memory optimization hai. Iska matlab ye nahi ki Java String ka public length() behavior change ho gaya.

24. String Performance – Which Approach Should You Use?
Ab practical development point of view se samjhte hain.
Scenario	Recommended
Normal text	String
Username / email	String
API response	String
Text comparison	equals()
Repeated concatenation	StringBuilder
Large dynamic text	StringBuilder
Shared synchronized mutable buffer	StringBuffer, when appropriate
Password handling	Prefer char[] where API supports it
Regex validation	Pattern / Matcher
Text splitting	split()
Multiline SQL/JSON template	Text Blocks

Example: Building SQL Query
Suppose tum dynamic SQL query construct kar rahe ho.
StringBuilder query = new StringBuilder();

query.append("SELECT * FROM employees ");

query.append("WHERE department = 'IT' ");

query.append("AND status = 'ACTIVE'");

System.out.println(query);


Output:
SELECT * FROM employees WHERE department = 'IT' AND status = 'ACTIVE'


However, production backend mein user input ko directly SQL String mein concatenate mat karna. SQL injection avoid karne ke liye PreparedStatement ya ORM parameter binding use karna chahiye.

25. Java Strings – Common Interview Questions
Ab interview preparation ke liye important questions aur unke crisp answers dekhte hain.
Q1. Is String a primitive or reference type in Java?
String is a reference type and a class in java.lang.

Q2. Why is String immutable in Java?
Security, String Pool optimization, thread safety and hash code caching.

Q3. What is String Pool?
A special pool of Strings maintained within heap memory to reuse String literals.

Q4. Difference between == and equals()?
== checks reference equality, whereas equals() checks content equality.

Q5. Difference between StringBuilder and StringBuffer?
Both are mutable. StringBuilder is not synchronized; StringBuffer has synchronized methods.

Q6. Can we extend String class?
No. String class is final.

Q7. Can we modify a String after creation?
No. String is immutable. Operations return new String results.

Q8. What does intern() do?
Returns the canonical String reference from the String Pool.

Q9. What is the difference between isEmpty() and isBlank()?
isEmpty() checks zero length. isBlank() also considers whitespace-only Strings blank.

Q10. What is the difference between length and length()?
Array uses length property, String uses length() method.

Q11. Is StringBuilder immutable?
No, it is mutable.

Q12. Is String thread-safe?
Its immutable content can safely be shared between threads. This does not automatically make every operation involving a String variable or surrounding state thread-safe.

Q13. What is the default capacity of StringBuilder?
16 characters when created without an initial capacity or content.

Q14. What is the difference between substring() and subSequence()?
substring() returns String, while subSequence() returns CharSequence.

Q15. What happens when you concatenate two String literals?
The Java compiler can evaluate constant String expressions at compile time.


26. Practical Coding Questions
Theory ke baad coding practice bahut important hai.

Ye questions tumhare Java fundamentals aur interview preparation dono ke liye useful rahenge.
Java String Practice Tracker
0/18



Beginner
[ ] Reverse a String without using reverse().
[ ] Check whether a String is palindrome.
[ ] Count vowels and consonants.
[ ] Count the number of words in a sentence.
[ ] Convert lowercase String to uppercase without using toUpperCase().

Intermediate
[ ] Find the first non-repeating character.
[ ] Find the frequency of each character.
[ ] Remove duplicate characters from a String.
[ ] Check whether two Strings are anagrams.
[ ] Find the longest word in a sentence.
[ ] Count occurrences of a particular word.
[ ] Reverse each word in a sentence.

Advanced
[ ] Find the longest substring without repeating characters.
[ ] Find the longest palindromic substring.
[ ] Implement your own substring search algorithm.
[ ] Compress a String using character frequencies.
[ ] Group anagrams from an array of Strings.
[ ] Find the minimum window substring.

Reset progress

27. Mini Project – Employee Name Processing
Ab ek chhota practical program banate hain jisme multiple String concepts ek saath use honge.

Requirement:

Suppose tumhare paas employees ke names hain.

Tumhe:
1. Names ko trim karna hai.
2. First letter uppercase karna hai.
3. Names ko comma-separated format mein combine karna hai.
4. Total employees count print karna hai.

import java.util.Arrays;

public class Main {

    public static void main(String[] args) {

        String[] employees = {
            " rahul ",
            "AMIT",
            " rishabh ",
            "priya"
        };

        StringBuilder result = new StringBuilder();

        for (int i = 0; i < employees.length; i++) {

            String name = employees[i].strip();

            name = name.substring(0, 1).toUpperCase()
                    + name.substring(1).toLowerCase();

            if (i > 0) {
                result.append(", ");
            }

            result.append(name);
        }

        System.out.println("Employees: " + result);

        System.out.println(
            "Total Employees: " + employees.length
        );
    }
}



Output:
Employees: Rahul, Amit, Rishabh, Priya
Total Employees: 4


Is single example mein tumne use kiya:
- String Array.
- strip().
- substring().
- toUpperCase().
- toLowerCase().
- String concatenation.
- StringBuilder.
- append().
- length.

28. Complete Java String Revision Sheet
Last mein ek quick revision table de raha hoon. Isko apne Java notes mein save kar sakte ho.
Topic	Important concept
String	Reference type / class
Immutability	Existing String content cannot change
String Pool	Reuses canonical String literals
new String()	Creates a separate String object
'==	Reference comparison
equals()	Content comparison
intern()	Gets canonical pooled reference
StringBuilder	Mutable, not synchronized
StringBuffer	Mutable, synchronized methods
append()	Add content
substring()	Extract text
split()	Convert text into String array
replaceAll()	Regex-based replacement
matches()	Match entire String against regex
parseInt()	String to integer
valueOf()	Convert values into String
char[]	Mutable character array
length()	UTF-16 code unit count
isEmpty()	Zero-length check
isBlank()	Empty or whitespace-only check
Text Blocks	Multiline Strings
Unicode	International text representation


29. What Should You Learn Next?
Ab tumne Java Strings ke almost saare important concepts cover kar liye hain.

Meri recommendation hai ki next OOP (Object-Oriented Programming) in Java start karo.
Your Next Java Learning Roadmap
1. Arrays in Java
   Covered
   1D, 2D, Jagged Arrays, Array operations
2. Strings in Java
   Covered
   String Pool, Immutability, StringBuilder, StringBuffer
3. OOP Fundamentals
   Next
   Class, Object, Constructor, this, static, final
4. Four Pillars of OOP
   
   Encapsulation, Inheritance, Polymorphism, Abstraction
5. Java Collections Framework
   
   List, Set, Map, ArrayList, HashMap, LinkedList
6. Exception Handling
   
   try-catch, finally, throw, throws, custom exceptions
7. Java 8+ Features
   
   Lambda, Functional Interfaces, Streams, Optional
8. Spring Boot
   
   DI, IoC, REST API, JPA, Hibernate, Security

Ek important suggestion: OOP start karne se pehle String ke beginner aur intermediate coding questions solve kar lena. Especially palindrome, anagram, character frequency aur first non-repeating character.

Ye concepts tumhe Java Collections, Streams aur Spring Boot ke code ko samajhne mein bhi help karenge.

Next hum OOP in Java – Complete Beginner to Advanced, JavaScript comparison ke saath isi learning style mein kar sakte hain.
