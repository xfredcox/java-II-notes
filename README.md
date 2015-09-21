# Java Programmer Level II

## Section 1: Java Class Design

### Item 1: Use access modifiers: private, protected, public.

- There are two levels of access control
  - Top level (public or package-private)
  - Member level (public, private, protected or package-private)

- Always default to package-private if access modifier not specified.

- Therefore:
  - If a class is not declared public, it is only visible within its package. Otherwise it is visible from anywhere.

- Memeber-level access:

Classes 	Class 	Package 	Subclass 	World
public 		Y 		Y 			Y 			Y
protected 	Y 		Y 			Y 			N
no modifier Y 		Y 			N 			N
private 	Y 		N 			N 			N

- Subclassing can allow greater access, but never less.

### Item 2 & 3: Override methods & Overload constructors and other methods appropriately.

- Overrideing requires:
  - Subclass
  - Identical signature (name + parameters)
  - Return type identical or covariant
- Good practice to use the @Overrride annotation

- Rules for resolving multiple inherited methods:
  - Instance methods are preferred over interface default methods
  - Methods already overriden by other candidates are ignored.

- You can't override a static/instance method with a instance/static method.

- Naming conventions: Lower-case or camel-case verbs.

- If your method overrides one of its superclass's methods, you can invoke the overridden method through the use of the keyword super. (Also accesses hidden methods (static methods))

``java
public class Subclass extends Superclass {
    // overrides printMethod in Superclass
    public void printMethod() {
        super.printMethod();
        System.out.println("Printed in Subclass");
    }
    public static void main(String[] args) {
        Subclass s = new Subclass();
        s.printMethod();    
    }
}
``

- You can also use super() to acces the superclass constructor.
- If a constructor does not explicitly invoke a superclass constructor, the Java compiler automatically inserts a call to the no-argument constructor of the superclass. 
- If the super class does not have a no-argument constructor, you will get a compile-time error.

### Item 4: Use the instanceof operator and casting.

``java
if (obj instanceof MountainBike) {
    MountainBike myBike = (MountainBike)obj;
}
``

### Item 5: Use virtual method invocation.

> The dictionary definition of polymorphism refers to a principle in biology in which an organism or species can have many different forms or stages


> The Java virtual machine (JVM) calls the appropriate method for the object that is referred to in each variable. _It does not call the method that is defined by the variable's type_. This behavior is referred to as virtual method invocation and demonstrates an aspect of the important polymorphism features in the Java language.

### Item 6: Override methods from the Object class to improve the functionality of your class.

See items 2 and 3.

### Item 7: Use package and import statements.

``java
package graphics;
public interface Draggable {
    . . .
}
``

``java
import graphics.Rectangle;
``

- If a member in one package shares its name with a member in another package and both packages are imported, you must refer to each member by its qualified name.

``java
import static java.lang.Math.PI;
``

----

## Section 2: Advanced Class Design

### Item 1: Identify when and how to apply abstract classes.

### Item 2: Construct abstract Java classes and subclasses.

### Item 3: Use the static and final keywords.

### Item 4: Create top-level and nested classes

### Item 5: Use enumerated types.

----

## Section 3: Object-Oriented Design Patterns

### Item 1: Write code that declares, implements and/or extends interfaces.

#### Interface Declaration

- Interfaces, like Classes, default to *package-private* access by default.
- Interfaces can contain, abstract, default and static methods:
  - Abstract methods contain no braces (only semi-colon);
  - *default* modifier
  - *static* modifier
- All methods are public by default
- Only default and static methods can contain implementations.
- Constants are public, static, final by default.
- Interfaces can be empty (no methods / constants)

#### Interface Implementation

- Classes implement interfaces using the *implements* keyword (following the *extends* keyword by convention)
- *Implements* keyword accepts a comma separated list of interfaces.

#### Extending an Interface

- Adding abstract methods to interfaces can annoy developers who must implement the new methods to avoid breaking code.
- The recommendation is to plan ahead and implement the whole interface up front. Two alternatives include:
  - Implement a InterfacePlus interface which extends the base Interface.
  - Implement the new method with a default implementation.

### Item 2: Choose between interface inheritance and class inheritance.

### Item 3: Develop code that implements "is-a" and/or "has-a" relationships

### Item 4: Apply object composition principles.

### Item 5: Design a class using the Singleton design pattern.

### Item 6: Write code to implement the DAO pattern.

### Item 7: Design and create objects using a factory, and use factories from the API

## Section 4: Generic and Collections

### Item 1: Create a generic class
### Item 2: Use the diamond syntax to create a collection
### Item 3: Analyze the interoperability of collections that use raw types and generic types.

- Benefits of Generics:
  - Stronger type checking
  - Elimination of casts
  - Enable generic algorithms

``java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;
    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
``

- Common type parameter names:
  - E - Element (used extensively by the Java Collections Framework)
  - K - Key
  - N - Number
  - T - Type
  - V - Value
  - S,U,V etc. - 2nd, 3rd, 4th types

- Declaring parameterized type:

``java
Box<Integer> integerBox;
``

- Instantiating:

``java
 Box<Integer> integerBox = new Box<Integer>();
// or
Box<Integer> integerBox = new Box<>();
``

- You can substitute a type parameter with a parameterized type:

``java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
`` 

- Parameterized type is built from the **raw type**

- You can instantiate raw types:

``java
Box rawBox = new Box();
``

- Raw type should be avoided because they leave unchecked warnings (which defeats the purpose of generics):

``java
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
//
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)
``

- Generic Methods:

``java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}
//
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
// or 
boolean same = Util.compare(p1, p2); //Infered type
``

- **Bound Type Parameters** allow you to enforce properties of the generics:

``java
public <U extends Number> void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }
``

- By enforcing this property, you can also use methods from the enforced parent class (ie. n.intValue())
- Multiple bounds are permited using the syntax <T extends B1 & B2 & B3>
  - Classes must be specified first.
  - Must be a subclass of all types listed.

> Given two concrete types A and B (for example, Number and Integer), MyClass<A> has no relationship to MyClass<B>, regardless of whether or not A and B are related. The common parent of MyClass<A> and MyClass<B> is Object.

``java
Map<String, List<String>> myMap = new HashMap(); 
// This raises an unchecked conversion warning because it lacks the diamond
``

### Item 4: Use wrapper classes and autoboxing.

#### Numbers Methods

- byte byteValue()
- short shortValue()
- int intValue()
- long longValue()
- float floatValue()
- double doubleValue()
- int compareTo(Byte anotherByte)
- int compareTo(Double anotherDouble)
- int compareTo(Float anotherFloat)
- int compareTo(Integer anotherInteger)
- int compareTo(Long anotherLong)
- int compareTo(Short anotherShort)
- boolean equals(Object obj) // must be of the same type to be true.

#### Integer Methods

- static Integer decode(String s)
- static int parseInt(String s)
- static int parseInt(String s, int radix)
- String toString()
- static String toString(int i)
- static Integer valueOf(int i)
- static Integer valueOf(String s)
- static Integer valueOf(String s, int radix)

### Item 5: Create and use a List, a Set, and a Deque.

#### Lists

- Ordered Collection (sequence)
- May contain duplicates
- Additional methods allow:
  - Positional access (get, set, add, addAll, remove)
  - Search (indexOf / lastIndexOf)
  - Iteration (next, previous, hasNext, listIterator)
  - Range-view (subList)
- Implementations:
  - General Purpose
    - ArrayList
      - Constant time positional access
    - LinkedList
      - Faster when constantly adding elements to the beggining of the list or iterating over elements to delete them. 
      - However, linear-time positional access.

  - Special Purpose
    - CopyOnWriteArrayList

- Two lists are equal if they contain the same elements in the same order.

- List swap method:

``java
public static <E> void swap(List<E> a, int i, int j) {
    E tmp = a.get(i);
    a.set(i, a.get(j));
    a.set(j, tmp);
}
``

#### Sets

- Same interface as Collection, with the restriction that all elements are unique.

- Implementations:
  - General Purpose:
    - HashSet
      - Fastest implementation, stores elements in a hash table. No guarantees of order.
    - TreeSet
      - Stores elements in red-black tree in order of their values.
    - LinkedHashset
      - Stores elements in a hash table with a linked list running through them. Returns the elements in the order they are inserted
  - Special-Purpose:
    - EnumSet

- Deduplication

``java
Collection<Type> noDups = new HashSet<Type>(c);
``

- Basic Operations
  - size
  - add
  - isEmpty
- Bulk Operations
  - containsAll
  - addAll
  - retainAll
  - removeAll

- All operations are done in place (destructive)

- Default initial capacity is 16.

- List shuffle method:

``java
public static void shuffle(List<?> list, Random rnd) {
    for (int i = list.size(); i > 1; i--)
        swap(list, i - 1, rnd.nextInt(i));
}
``

#### Queue

- Typically FIFO, but may implement custom ordering.
  - The head is always the next element to get pulled. There is no such thing asa tail in a custom ordering queue (ie. PriorityQueue)

- Interface:
  - Insert:
    - offer(E e) 
    - add() [as from collection parent class]
    - Includes the element in the queue. If it exceeds the queue capcity, add throws an exception while offer returns false.
  - Remove:
    - poll()
    - remove()
    - Methods pull from the queue head. Remove throw an exception is no elements exist. Poll returns null if no elements exist. 
  - Examine:
    - peek() 
    - element()
    - Methods inspect, but do not remove, the queue head. Element throws an exception is no head exists, peek returns null.

- Queues do not usually implement element-based equals/hashCode but instead rely on the Object implementation.

- Implementations
  - General Purpose
    - LikendList
    - PriorityQueue
  - Concurrent Queues (BlockingQueue extends Queue)
    - LinkedBlockingQueue (bounded)
    - ArrayBlockingQueue (bounded)
    - PriorityBlockingQueue (unbounded)
    - DealyQueue
    - SynchronousQueue

### Item 6: Create and use a Map

- Implementations
  - General Purpose
    - HashMap
    - TreeMap
    - LinkedHashMap
  - Special Purpose:
    - EnumMap
    - WeakHashMap
    - IdentityHashMap
  - Concurrent Implementations (ConcurrentMap Interface)
    - ConcurrentHashMap

- Defaultdict implementation:

``java
static <K, V> Map<K, V> newAttributeMap(Map<K, V>defaults, Map<K, V> overrides) {
    Map<K, V> result = new HashMap<K, V>(defaults);
    result.putAll(overrides);
    return result;
}
``

- Collection Views
  - keySet
  - values
  - entrySet

- All Map iteration occurs throught collection view:

``java
for (KeyType key : m.keySet())
    System.out.println(key);
``

``java
for (Map.Entry<KeyType, ValType> e : m.entrySet())
    System.out.println(e.getKey() + ": " + e.getValue());
``

### Item 7: Use java.util.Comparator and java.lang.Comparable.

- Collections.sort(l) and  Collections.sort(list, comparator)
- If you try to sort a list, the elements of which do not implement Comparable, Collections.sort(list) will throw a ClassCastException.

``java
public interface Comparable<T> {
    public int compareTo(T o);
}
``

vs. 

``java
public interface Comparator<T> {
    int compare(T o1, T o2);
}
``

- Comparable implements an objects relative comparison. A comparator implements a *non-natural* comparison rule.

### Item 8: Sort and search arrays and lists.

- Polymorphic algorithms
  - Sorting:
    - *sort* method is a fast (n * log(n)) and stable *merge sort* implementation. They decided not to use quicksort because it is unstable.


``java
import java.util.*;
public class Sort {
    public static void main(String[] args) {
        List<String> list = Arrays.asList(args);
        Collections.sort(list);
        System.out.println(list);
    }
}
``

    - sort w/ comparator

``java
Collections.sort(winners, new Comparator<List<String>>() {
    public int compare(List<String> o1, List<String> o2) {
        return o2.size() - o1.size();
    }});
``

  - Shuffling (shuffle)

``java
Collections.shuffle(deck);
``

  - Reverse (reverse)
  - Fill (fill)
  - Copy (copy)
  - Swap (swap)
  - addAll (addAll)
  - Search
    - binarySearch (assumes ordered List)

``java
    int pos = Collections.binarySearch(list, key);
if (pos < 0)
   l.add(-pos-1, key);
``

    - If not found, returns: (-(insertion point) - 1)

  - frequency
  - disjoint (whether two elements contain no elements in common)
  - min / max

- SortedSet Interface

``java
public interface SortedSet<E> extends Set<E> {
    // Range-view
    SortedSet<E> subSet(E fromElement, E toElement);
    SortedSet<E> headSet(E toElement);
    SortedSet<E> tailSet(E fromElement);
    // Endpoints
    E first();
    E last();
    // Comparator access
    Comparator<? super E> comparator();
}
``

- SortedMap Interface

``java
public interface SortedMap<K, V> extends Map<K, V>{
    Comparator<? super K> comparator();
    SortedMap<K, V> subMap(K fromKey, K toKey);
    SortedMap<K, V> headMap(K toKey);
    SortedMap<K, V> tailMap(K fromKey);
    K firstKey();
    K lastKey();
}
``

## Section 5: String Processing

### Item 1: Search, parse and build strings.

- The String object has 13 contructors:

``java
String greeting = "Hello world!";
``

``java
char[] helloArray = { 'h', 'e', 'l', 'l', 'o', '.' };
String helloString = new String(helloArray);
``

- Strings are immutable.
- length / concat / charAt
- concat accepts "+"
- getChars(i, n, temmpCharArray, 0) (transforms a substring to an array)
- String Formatting

``java
String fs;
fs = String.format("The value of the float " +
                   "variable is %f, while " +
                   "the value of the " + 
                   "integer variable is %d, " +
                   " and the string is %s",
                   floatVar, intVar, stringVar);
System.out.println(fs);
``

- Converting strings to Numbers:
  - Each Number subclass contains a *valueOf* method which converts strings to that type.

- Converting Numbers to String:
  - Concatenate with a string.
  - String.valueOf(i)
  - Integer.toString(i)

- Methods for comparing strings:
  - endsWith / startsWith
  - compareTo / compareToIgnoreCase
  - equals / equalsIgnoreCase
  - boolean regionMatches(int toffset, String other, int ooffset, int len)
  - boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)
  - boolean matches(String regex)

- Substring Methods
  - String substring(int beginIndex, int endIndex)
  - String substring(int beginIndex)
  - String[] split(String regex)
  - String[] split(String regex, int limit)
  - CharSequence subSequence(int beginIndex, int endIndex)
  - String trim()
  - String toLowerCase()
  - String toUpperCase()

- String Search
  - int indexOf(int ch)
  - int lastIndexOf(int ch)
  - int indexOf(int ch, int fromIndex)
  - int lastIndexOf(int ch, int fromIndex)
  - int indexOf(String str)
  - int lastIndexOf(String str)
  - int indexOf(String str, int fromIndex)
  - int lastIndexOf(String str, int fromIndex)
  - boolean contains(CharSequence s)

- CharSequence is an Interface implemented by the String Object.

- Replace Methods
  - String replace(char oldChar, char newChar)
  - String replace(CharSequence target, CharSequence replacement)
  - String replaceAll(String regex, String replacement)
  - String replaceFirst(String regex, String replacement)

### Item 2: Search, parse, and replace strings by using regular expressions.

- java.util.regex
  - Pattern
    - A Pattern object is a compiled representation of a regular expression. The Pattern class provides no public constructors. To create a pattern, you must first invoke one of its public static compile methods, which will then return a Pattern object. These methods accept a regular expression as the first argument.

  - Matcher
    - A Matcher object is the engine that interprets the pattern and performs match operations against an input string. Like the Pattern class, Matcher defines no public constructors. You obtain a Matcher object by invoking the matcher method on a Pattern object.

  - PatternSyntaxException

``java
Pattern pattern = Pattern.compile(console.readLine("%nEnter your regex: "));
Matcher matcher = pattern.matcher(console.readLine("Enter input string to search: "));
``

- Pattern modifier flags:

``java
Pattern pattern = Pattern.compile(console.readLine("%nEnter your regex: "), Pattern.CASE_INSENSITIVE);
// or
Pattern pattern = Pattern.compile("[az]$", Pattern.MULTILINE | Pattern.UNIX_LINES);
// or as Embedded Flag Expressions
Pattern pattern = Pattern.compile("(?i)foo");
``

  - For embedded flag expressions:
    - Pattern.CANON_EQ  None
    - Pattern.CASE_INSENSITIVE  (?i)
    - Pattern.COMMENTS  (?x)
    - Pattern.MULTILINE (?m)
    - Pattern.DOTALL  (?s)
    - Pattern.LITERAL None
    - Pattern.UNICODE_CASE  (?u)
    - Pattern.UNIX_LINES  (?d)

- Pattern.matches method

``java
Pattern.matches("\\d","1");
``

- Pattern.split method

``java
Pattern p = Pattern.compile(REGEX);
String[] items = p.split(INPUT);
``

- Pattern.quote(String) method
  - Returns a string regex that would match the argument (literalized)

- Pattern.toString() method

- Regex support in String object:
  - public boolean matches(String regex)
  - public String[] split(String regex)
  - public String replace(CharSequence target,CharSequence replacement)

### Item 3: Use string formatting.

``java
System.out.format("The value of " + "the float variable is " +
     "%f, while the value of the " + "integer variable is %d, " +
     "and the string is %s", floatVar, intVar, stringVar); 
``

> The first parameter, format, is a format string specifying how the objects in the second parameter, args, are to be formatted. The format string contains plain text as well as format specifiers, which are special characters that format the arguments of Object... args.

- Converters and Flags Used in TestFormat.java
  - d   A decimal integer.
  - f   A float.
  - n   A new line character appropriate to the platform running the application. You should always use %n, rather than \n.
  - tB    A date & time conversion—locale-specific full name of month.
  - td, te    A date & time conversion—2-digit day of month. td has leading zeroes as needed, te does not.
  - ty, tY    A date & time conversion—ty = 2-digit year, tY = 4-digit year.
  - tl    A date & time conversion—hour in 12-hour clock.
  - tM    A date & time conversion—minutes in 2 digits, with leading zeroes as necessary.
  - tp    A date & time conversion—locale-specific am/pm (lower case).
  - tm    A date & time conversion—months in 2 digits, with leading zeroes as necessary.
  - tD    A date & time conversion—date as %tm%td%ty
  - 08  Eight characters in width, with leading zeroes as necessary.
  - + Includes sign, whether positive or negative.
  - , Includes locale-specific grouping characters.
  - - Left-justified..
  - .3  Three places after decimal point.
  - 10.3  Ten characters in width, right justified, with three places after decimal point.


- DecimalFormat Class

``java
DecimalFormat myFormatter = new DecimalFormat(pattern);
String output = myFormatter.format(value);
``

- DecimalFormat.java Output
  - 123456.789 -> ###,###.### -> 123,456.789 :: The pound sign (#) denotes a digit, the comma is a placeholder for the grouping separator, and the period is a placeholder for the decimal separator.
  - 123456.789 -> ###.## -> 123456.79 :: The value has three digits to the right of the decimal point, but the pattern has only two. The format method handles this by rounding up.
  - 123.78 -> 000000.000 -> 000123.780 :: The pattern specifies leading and trailing zeros, because the 0 character is used instead of the pound sign (#).
  - 12345.67 -> $###,###.### -> $12,345.67 :: The first character in the pattern is the dollar sign ($). Note that it immediately precedes the leftmost digit in the formatted output.


## Section 6: Exceptions and Assertions

### Item 1: Use throw and throws statements.

``java
public void writeList() throws IOException, IndexOutOfBoundsException {}
``

``java
throw new EmptyStackException();
``

### Item 2: Use the try statement with multi-catch, and finally clauses.

### Item 3: Autoclose resources with a try-with-resources statement.

``java
static String readFirstLineFromFile(String path) throws IOException {
    try (BufferedReader br =
                   new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
``

- Any class that implements java.lang.AutoCloseable or java.io.Closeable

> See the Javadoc of the AutoCloseable and Closeable interfaces for a list of classes that implement either of these interfaces. The Closeable interface extends the AutoCloseable interface. The close method of the Closeable interface throws exceptions of type IOException while the close method of the AutoCloseable interface throws exceptions of type Exception. Consequently, subclasses of the AutoCloseable interface can override this behavior of the close method to throw specialized exceptions, such as IOException, or no exception at all.

### Item 4: Create custom exceptions.

### Item 5: Test invariants by using assertions.

## Section 7: Java I/O Fundamentals

### Item 1: Read and write data from the console.

- I/O Streams
  - Byte Streams 
    - All children of InputStream / Output Stream
    - Examples: java.io.FileInputStream / java.io.FileOutputStream
    - Building block of all other streams.
  - Character Streams
    -  All children of Reader / Writer.
    - Examples: java.io.FileReader / java.io.
    - Wrapper arounf Byte Stream
  - Buffered Streams

> To reduce this kind of overhead, the Java platform implements buffered I/O streams. Buffered input streams read data from a memory area known as a buffer; the native input API is called only when the buffer is empty. Similarly, buffered output streams write data to a buffer, and the native output API is called only when the buffer is full.

``java
inputStream = new BufferedReader(new FileReader("xanadu.txt"));
outputStream = new BufferedWriter(new FileWriter("characteroutput.txt"));
``
> It often makes sense to write out a buffer at critical points, without waiting for it to fill. This is known as flushing the buffer.

- I/O From the Command Line

  - Standard Streams

> The Java platform supports three Standard Streams: Standard Input, accessed through System.in; Standard Output, accessed through System.out; and Standard Error, accessed through System.err. 

    - For historical reasons, these are byte (not character) streams.

``java
InputStreamReader cin = new InputStreamReader(System.in)
``

  - The Console

> Before a program can use the Console, it must attempt to retrieve the Console object by invoking System.console(). If the Console object is available, this method returns it. If System.console returns NULL, then Console operations are not permitted, either because the OS doesn't support them or because the program was launched in a noninteractive environment.

> The Console object supports secure password entry through its readPassword method. This method helps secure password entry in two ways. First, it suppresses echoing, so the password is not visible on the user's screen. _Second, readPassword returns a character array, not a String, so the password can be overwritten, removing it from memory as soon as it is no longer needed._

- Data Streams

> Data streams support binary I/O of primitive data type values (boolean, char, byte, short, int, long, float, and double) as well as String values. All data streams implement either the DataInput interface or the DataOutput interface. This section focuses on the most widely-used implementations of these interfaces, DataInputStream and DataOutputStream.

- Object Streams 

  - Transmits objects that implement the Serializable interface.

``java
Object ob = new Object();
out.writeObject(ob);
out.writeObject(ob);
//
Object ob1 = in.readObject();
Object ob2 = in.readObject();
``

> This results in two variables, ob1 and ob2, that are references to a single object.However, if a single object is written to two different streams, it is effectively duplicated — a single program reading both streams back will see two distinct objects.

### Item 2: Use streams to read and write files.

- Path Class

``java
Path p1 = Paths.get("/tmp/foo");
Path p2 = Paths.get(args[0]);
Path p3 = Paths.get(URI.create("file:///Users/joe/FileTest.java"));
``

``java
System.out.format("toString: %s%n", path.toString());
System.out.format("getFileName: %s%n", path.getFileName());
System.out.format("getName(0): %s%n", path.getName(0));
System.out.format("getNameCount: %d%n", path.getNameCount());
System.out.format("subpath(0,2): %s%n", path.subpath(0,2));
System.out.format("getParent: %s%n", path.getParent());
System.out.format("getRoot: %s%n", path.getRoot());
``

``java
System.out.format("%s%n", p1.toUri());
``

``java
Path fullPath = inputPath.toAbsolutePath();
``

``java
try {
    Path fp = path.toRealPath();
} catch (NoSuchFileException x) {
    System.err.format("%s: no such" + " file or directory%n", path);
    // Logic for case when file doesn't exist.
} catch (IOException x) {
    System.err.format("%s%n", x);
    // Logic for other sort of file error.
}
``

> This method throws an exception if the file does not exist or cannot be accessed. You can catch the exception when you want to handle any of these cases. 

``java
Path p1 = Paths.get("/home/joe/foo");
// Result is /home/joe/foo/bar
System.out.format("%s%n", p1.resolve("bar"));
``

``java
Path p1 = Paths.get("home");
Path p3 = Paths.get("home/sally/bar");
// Result is sally/bar
Path p1_to_p3 = p1.relativize(p3);
// Result is ../..
Path p3_to_p1 = p3.relativize(p1);
``

> The Path class implements the Iterable interface. The iterator method returns an object that enables you to iterate over the name elements in the path.

- Catching Exceptions

> All methods that access the file system can throw an IOException. It is best practice to catch these exceptions by embedding these methods into a try-with-resources statement

``java
Charset charset = Charset.forName("US-ASCII");
String s = ...;
try (BufferedWriter writer = Files.newBufferedWriter(file, charset)) {
    writer.write(s, 0, s.length());
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
``

> In addition to IOException, many specific exceptions extend FileSystemException. This class has some useful methods that return the file involved (getFile), the detailed message string (getMessage), the reason why the file system operation failed (getReason), and the "other" file involved, if any (getOtherFile).

``java
try (...) {
    ...    
} catch (NoSuchFileException x) {
    System.err.format("%s does not exist\n", x.getFile());
}
``

- Glob Pattern Matching

  - * :: Wildcard
  - ** :: Wildcard accross directories
  - ? :: exactly one character
  - {sun,moon,stars} :: collections of subpatterns
  - [a-z,A-Z]
  - [aeiou]

- File Class Static Methods
  - File.exists(f1)
  - File.notExists(f1)
  - File.isSameFile(p1, p2)
  - File.isReadable(f1)
  - File.isExecutable(f1)
  - File.isRegularFile(f1)
  - File.delete(f1)
  - File.deleteIsExists(f1)
  - File.move(p1, p2, CopyOptions...)
  - File.copy(p1, p2, CopyOptions...)

``java
import static java.nio.file.StandardCopyOption.*;
...
Files.copy(source, target, REPLACE_EXISTING);
``
    - Options include REPLACE_EXISTING, COPY_ATTRIBUTES and NOFOLLOW_LINKS

  - File.size(p1)
  - File.isDirectory(p1, LinkOptions)
  - File.isRegularFile(p1, LinkOptions...)
  - File.isRegularFile(p1, LinkOptions...)
  - File.isHidden(p1)
  - File.getLastModifiedTime(p1, LinkOption...)
  - File.setLastModifiedTime(p1, FileTime)
  - File.getOwner(Path, LinkOption...)
  - File.setOwner(Path, UserPrincipal)

#### IO

- Commonly Used Methods for Small Files

``java
// READ
Path file = ...;
byte[] fileArray;
fileArray = Files.readAllBytes(file);
// WRITE
Path file = ...;
byte[] buf = ...;
Files.write(file, buf);
``

- Buffered I/O Methods for Text Files

``java
// READ
Charset charset = Charset.forName("US-ASCII");
try (BufferedReader reader = Files.newBufferedReader(file, charset)) {
    String line = null;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
// WRITE
Charset charset = Charset.forName("US-ASCII");
String s = ...;
try (BufferedWriter writer = Files.newBufferedWriter(file, charset)) {
    writer.write(s, 0, s.length());
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
``

- Methods for Unbuffered Streams and Interoperable with java.io APIs

``java
// READ
Path file = ...;
try (InputStream in = Files.newInputStream(file);
    BufferedReader reader =
      new BufferedReader(new InputStreamReader(in))) {
    String line = null;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException x) {
    System.err.println(x);
}
// WRITE
try (OutputStream out = new BufferedOutputStream(
      Files.newOutputStream(p, CREATE, APPEND))) {
      out.write(data, 0, data.length);
    } catch (IOException x) {
      System.err.println(x);
    }
``

- Methods for Channels and ByteBuffers

- Methods for Creating Regular and Temporary Files

``java
Path file = ...;
try {
    // Create the empty file with default permissions, etc.
    Files.createFile(file);
} catch (FileAlreadyExistsException x) {
    System.err.format("file named %s" +
        " already exists%n", file);
} catch (IOException x) {
    // Some other sort of failure, such as permissions.
    System.err.format("createFile error: %s%n", x);
}
``

``java
try {
    Path tempFile = Files.createTempFile(null, ".myapp");
    System.out.format("The temporary file" +
        " has been created: %s%n", tempFile)
;
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
``

#### Random Access File

> The SeekableByteChannel interface extends channel I/O with the notion of a current position. Methods enable you to set or query the position, and you can then read the data from, or write the data to, that location.

- Methods
  - position :: Return the channel position
  - position(long) :: Set the channel position
  - read(ByteBuffer) :: Reads bytes from the buffer to the channel
  - write(ByteBuffer)
  - truncate(long) :: Truncate the file connected to the channel.

#### File Sytems

``java
Iterable<Path> dirs = FileSystems.getDefault().getRootDirectories();
for (Path name: dirs) {
    System.err.println(name);
}
``

``java
Path dir = ...;
Files.createDirectory(path);
// or
Set<PosixFilePermission> perms =
    PosixFilePermissions.fromString("rwxr-x---");
FileAttribute<Set<PosixFilePermission>> attr =
    PosixFilePermissions.asFileAttribute(perms);
Files.createDirectory(file, attr);
``

``java
// Lists files in directory
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir)) {
    for (Path file: stream) {
        System.out.println(file.getFileName());
    }
} catch (IOException | DirectoryIteratorException x) {
    // IOException can never be thrown by the iteration.
    // In this snippet, it can only be thrown by newDirectoryStream.
    System.err.println(x);
}
``

``java
// Glob filtering of directory files
Path dir = ...;
try (DirectoryStream<Path> stream =
     Files.newDirectoryStream(dir, "*.{java,class,jar}")) {
    for (Path entry: stream) {
        System.out.println(entry.getFileName());
    }
} catch (IOException x) {
    // IOException can never be thrown by the iteration.
    // In this snippet, it can // only be thrown by newDirectoryStream.
    System.err.println(x);
}
``

#### Links

- Hard Links
  - The target must exist
  - Not allowed on directories
  - Not allowed accross partitions
  - Identical metadata

``java
// Symlink
Path newLink = ...;
Path target = ...;
try {
    Files.createSymbolicLink(newLink, target);
} catch (IOException x) {
    System.err.println(x);
} catch (UnsupportedOperationException x) {
    // Some file systems do not support symbolic links.
    System.err.println(x);
}
// Hard Link
Path newLink = ...;
Path existingFile = ...;
try {
    Files.createLink(newLink, existingFile);
} catch (IOException x) {
    System.err.println(x);
} catch (UnsupportedOperationException x) {
    // Some file systems do not
    // support adding an existing
    // file to a directory.
    System.err.println(x);
}
``

``java
Path file = ...;
boolean isSymbolicLink = Files.isSymbolicLink(file);
``

``java
Path file = ...;
boolean isSymbolicLink = Files.isSymbolicLink(file);
``

#### FileVisitor Interface

- Interface
  - preVisitDirectory – Invoked before a directory's entries are visited.
  - postVisitDirectory – Invoked after all the entries in a directory are visited. If any errors are encountered, the specific exception is passed to the method.
  - visitFile – Invoked on the file being visited. The file's BasicFileAttributes is passed to the method, or you can use the file attributes package to read a specific set of attributes. For example, you can choose to read the file's DosFileAttributeView to determine if the file has the "hidden" bit set.
  - visitFileFailed – Invoked when the file cannot be accessed. The specific exception is passed to the method. You can choose whether to throw the exception, print it to the console or a log file, and so on.

``java
Path startingDir = ...;
PrintFiles pf = new PrintFiles();
Files.walkFileTree(startingDir, pf);
``

> A file tree is walked depth first, but you cannot make any assumptions about the iteration order that subdirectories are visited.

> The FileVisitor methods return a FileVisitResult value. You can abort the file walking process or control whether a directory is visited by the values you return in the FileVisitor methods.

- Return values:
  - CONTINUE
  - TERMINATE
  - SKIP_SUBTREE
  - SKIP_SIBLINGS

#### PathMatcher Class

``java
String pattern = ...;
PathMatcher matcher =
    FileSystems.getDefault().getPathMatcher("glob:" + pattern);
``

> The string argument passed to getPathMatcher specifies the syntax flavor and the pattern to be matched. 

``java
Path filename = ...;
if (matcher.matches(filename)) {
    System.out.println(filename);
}
``

#### Watching File Changes

> The java.nio.file package provides a file change notification API, called the Watch Service API. 

``java
WatchService watcher = FileSystems.getDefault().newWatchService();
``

``java
import static java.nio.file.StandardWatchEventKinds.*;
Path dir = ...;
try {
    WatchKey key = dir.register(watcher,
                           ENTRY_CREATE,
                           ENTRY_DELETE,
                           ENTRY_MODIFY);
} catch (IOException x) {
    System.err.println(x);
}
``

#### Determining MIME Type

``java
try {
    String type = Files.probeContentType(filename);
    if (type == null) {
        System.err.format("'%s' has an" + " unknown filetype.%n", filename);
    } else if (!type.equals("text/plain") {
        System.err.format("'%s' is not" + " a plain text file.%n", filename);
        continue;
    }
} catch (IOException x) {
    System.err.println(x);
}
``

#### List mounted devices

``java
for (FileStore store: FileSystems.getDefault().getFileStores()) {
   ...
}


### Section 10: Threads

> Processing time for a single core is shared among processes and threads through an OS feature called _time slicing_.

- **Processes**
  - Have their own execution environment (incl. memory)
  - Pipes & Sockets == Inter Process Communication (IPC)
  - Java creates additional processes using *ProcessBuilder*

- **Threads**
  - *lightweight processes*, threads exist within a process
  - Share resources, incl memory and files

``java
public class HelloRunnable implements Runnable {
    public void run() {
        System.out.println("Hello from a thread!");
    }
    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }
}
// OR
public class HelloThread extends Thread {
    public void run() {
        System.out.println("Hello from a thread!");
    }
    public static void main(String args[]) {
        (new HelloThread()).start();
    }

}
``

``java
//Pause for 4 seconds
Thread.sleep(4000);
``

> Sleep throws InterruptedExceptionwhen another thread interrupts the current thread while sleep is active.

> A thread sends an interrupt by invoking interrupt on the Thread object for the thread to be interrupted. For the interrupt mechanism to work correctly, the interrupted thread must support its own interruption.

> What if a thread goes a long time without invoking a method that throws InterruptedException? Then it must periodically invoke Thread.interrupted, which returns true if an interrupt has been received.

> The join method allows one thread to wait for the completion of another.

- Synchronization:
  - (-) *thread interference*

  > Interference happens when two operations, running in different threads, but acting on the same data, interleave. This means that the two operations consist of multiple steps, and the sequences of steps overlap.

  - (-) *memory consistency*

  > Memory consistency errors occur when different threads have inconsistent views of what should be the same data

  - (+) *thread contention*

> two or more threads try to access the same resource simultaneously and cause the Java runtime to execute one or more threads more slowly, or even suspend their execution

- Synchronization idioms:

> Synchronization is built around an internal entity known as the intrinsic lock or monitor lock. (The API specification often refers to this entity simply as a "monitor.") 
>  The lock release occurs even if the return was caused by an uncaught exception.

> You might wonder what happens when a static synchronized method is invoked, since a static method is associated with a class, not an object. In this case, the thread acquires the intrinsic lock for the Class object associated with the class. Thus access to class's static fields is controlled by a lock that's distinct from the lock for any instance of the class.

  - Synchronization methods

``java
public synchronized void increment() {
  c++;
}
``

> It is not possible for two invocations of synchronized methods on the same object to interleave. When one thread is executing a synchronized method for an object, all other threads that invoke synchronized methods for the same object block (suspend execution) until the first thread is done with the object.

> When a synchronized method exits, it automatically establishes a happens-before relationship with any subsequent invocation of a synchronized method for the same object. This guarantees that changes to the state of the object are visible to all threads.

> Note that constructors cannot be synchronized — using the synchronized keyword with a constructor is a syntax error. 

  - Synchronization statements

``java
public void addName(String name) {
    synchronized(this) {
        lastName = name;
        nameCount++;
    }
    nameList.add(name);
}
``

``java
// All updates of these fields must be synchronized, but there's no reason to prevent an update of c1 from being interleaved with an update of c2 — and doing so reduces concurrency by creating unnecessary blocking.
public class MsLunch {
    private long c1 = 0;
    private long c2 = 0;
    private Object lock1 = new Object();
    private Object lock2 = new Object();
    public void inc1() {
        synchronized(lock1) {
            c1++;
        }
    }
    public void inc2() {
        synchronized(lock2) {
            c2++;
        }
    }
}
``

- **Atomic Access**

> Reads and writes are atomic for reference variables and for most primitive variables (all types except long and double).

> Reads and writes are atomic for all variables declared volatile (including long and double variables).

> Changes to a volatile variable are always visible to other threads. What's more, it also means that when a thread reads a volatile variable, it sees not just the latest change to the volatile, but also the side effects of the code that led up the change.

> Using simple atomic variable access is more efficient than accessing these variables through synchronized code, but requires more care by the programmer to avoid memory consistency errors

- **Deadlock**

> Deadlock describes a situation where two or more threads are blocked forever, waiting for each other.

- **Starvation**

> Starvation describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress.

- **Livelock**

> A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then livelock may result. As with deadlock, livelocked threads are unable to make further progress. However, the threads are not blocked — they are simply too busy responding to each other to resume work.

#### Guarded Blocks

- Object.wait is less wastefull thank while looping.

``java
public synchronized void guardedJoy() {
    // This guard only loops once for each special event, which may not
    // be the event we're waiting for.
    while(!joy) {
        try {
            wait();
        } catch (InterruptedException e) {}
    }
    System.out.println("Joy and efficiency have been achieved!");
}
``

> Always invoke wait inside a loop that tests for the condition being waited for. Don't assume that the interrupt was for the particular condition you were waiting for, or that the condition is still true.

- You can wake up the waiting thread with *notifyAll* of *notify*

``java
public synchronized notifyJoy() {
    joy = true;
    notifyAll();
}
``

- *notify* only informs one thread, but you can't specify which one.

#### Immutable Objects

- Rules for Immutability
  - Don't provide setter methods
  - Make all fields *final* and *private*
  - Don't allow subclasses to overrice methods (ie. *final* class OR *private* constructor with instantiation via factory)
  - Don't modify nested mutable instances.

### Executors

- Executor Interface

> The Executor interface provides a single method, execute, designed to be a drop-in replacement for a common thread-creation idiom.

``java
e.execute(r); // where r is Runnable
``

- ExecutorService

> Like execute, submit accepts Runnable objects, but also accepts Callable objects, which allow the task to return a value. The submit method returns a Future object, which is used to retrieve the Callable return value and to manage the status of both Callable and Runnable tasks.

- ScheduledExecutorService

> The ScheduledExecutorService interface supplements the methods of its parent ExecutorService with schedule, which executes a Runnable or Callable task after a specified delay.

> In addition, the interface defines scheduleAtFixedRate and scheduleWithFixedDelay, which executes specified tasks repeatedly, at defined intervals.

### TODO: Concurrent Collections

http://docs.oracle.com/javase/tutorial/essential/concurrency/collections.html

## Localization

### Creating a Locale Object

- Locale.Builder

``java
Locale aLocale = new Locale.Builder().setLanguage("fr").setRegion("CA").build()
``

- Locale Contructors

``java
aLocale = new Locale("fr", "CA");
bLocale = new Locale("en", "US");
cLocale = new Locale("en", "GB");
dLocale = new Locale("ru");
``

- Locale.forLanguageTag

> If you have a language tag string that conforms to the IETF BCP 47 standard, you can use the forLanguageTag(String) factory method.

``java
Locale aLocale = Locale.forLanguageTag("en-US");
Locale bLocale = Locale.forLanguageTag("ja-JP-u-ca-japanese");
``

> Note that the Locale API only requires that your language tag be syntactically well-formed. It does not perform any extra validation (such as checking to see if the tag is registered in the IANA Language Subtag Registry).

- Locale Constants

``java
j1Locale = Locale.JAPANESE;
``

### BCP47 Extensions

> 'u' specifies a Unicode locale extension, and 'x' specifies a private use extension.

### Identifying Available Locales

> You pass the Locale object to other objects, which then do the real work. These other objects, which we call locale-sensitive, do not know how to deal with all possible Locale definitions.

- getAvailableLocale

``java
import java.util.*;
import java.text.*;
public class Available {
    static public void main(String[] args) {
        Locale list[] = DateFormat.getAvailableLocales();
        for (Locale aLocale : list) {
            System.out.println(aLocale.toString());
        }
    }
}
``

### Language Ranges

``java
  // Create a Language Priority List
  String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
  List<Locale.LanguageRange> languageRanges = Locale.LanguageRange.parse(ranges)
``

### Language Preference Filtering

``java
package languagetagdemo;
import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;
public class LanguageTagDemo {
    public static void main(String[] args) {
        // Create a collection of Locale objects to filter
        Collection<Locale> locales = new ArrayList<>();
        locales.add(Locale.forLanguageTag("en-GB"));
        locales.add(Locale.forLanguageTag("ja"));
        locales.add(Locale.forLanguageTag("zh-cmn-Hans-CN"));
        locales.add(Locale.forLanguageTag("en-US"));
        // Express the user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List<Locale.LanguageRange> languageRanges = Locale.LanguageRange.parse(ranges);
        // Now filter the Locale objects, returning any matches
        List<Locale> results = Locale.filter(languageRanges,locales);
        // Print out the matches
        for(Locale l : results){
        System.out.println(l.toString());
        }
    }
}
``

``java
  // Find the BEST match, and return just one result
  Locale result = Locale.lookup(languageRanges,locales);
``

### Locale Scope

- Locale.getDefault (from JVM/OS)

### Implementation

> For example, if you would like to provide a NumberFormat object for a new locale, you have to implement the java.text.spi.NumberFormatProvider class.

``java
Locale loc = new Locale("da", "DK");
NumberFormat nf = NumberFormatProvider.getNumberInstance(loc);
``

# Questions

- What is the difference between the Integer decode and parseInt static methods? 
  - Decode returns an Integer and accepts decimal, octal or hexadecimal string representations. parseInt returns an int and only accepts decimal representation (though overloaded method accepts radix arg).

