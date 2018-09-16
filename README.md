# What's New in Java 8
## Introduction to lamda expression
## What is Lambda Expression for??
    Ans: To make instances of anonymous classes easier to write
    
## A First lambda Expression

```java
    FileFilter fileFilter = new FileFilter(){
      @Override
      public boolean accept(File file){
        return file.getName().endsWith(".java");
      }
    };
    // Done with just one line and it is more readable
    FileFilter filter = (File file) -> file.getName.endswith(".java");
```
## Full Example of Lambda Expression

```java
public class FirstLambda {
    public static void main(String[] args) {
        /*FileFilter filter = new FileFilter() {
            @Override
            public boolean accept(File file) {
                return file.getName().endsWith(".java");
            }
        };
        File dir = new File("H:\\Contest\\src\\march2");
        File[] files = dir.listFiles(filter);
        for (File f : files){
            System.out.println(f);
        }*/
        // do the above code with lambda Expression
        FileFilter fileFilter = (File file)->
                file.getName().endsWith(".java");
        File dir = new File("H:\\Contest\\src\\march2");
        File[] files = dir.listFiles(fileFilter);
        for (File f : files){
            System.out.println(f);
        }
    }
}
```
## If i have more than one argument:

```java
Comparator<String> c = (String s1, String s2) -> Integer.compare(s1.lenth(),s2.length());
```
## What is the type of a lambda expression?
    Ans: It is called a functional interface.Now a functional interface is an interface with only one abstract method.
    It is just here for convenience, the compiler can tell me whether the interface is functional or not . If i added this annotation on an interface, the annotation beging funing functinal interface.
for example
    
 ```java
    @FunctionalInterface
    public interface MyFunctionalInterface{
    oneMethod();
    }
 ```
 ## Can i put a lambda Expression in a variable?
 Ans: Yes i can put a lambda expression in a variable. It can be taken as a method parameter
 and can be returned by a method and moved around all my java application. 
 
## Is a lambda an Object?
### lets compare the following:

```java
Comparator<String> c = (String s1, String s2) -> Integer.compare(s1.lenth(),s2.length());

Comparetor<String> c = 
    new Comparator<String>(String s1, String s2){
    public boolean compareTo(String s1, String s2){
        Integer.compare(s1.length(), s2.length());
    }
    };
```    
Now What is the biggest difference between those two pieces of code? First one is a lambda
expression and second one is an instance of an annonymous class. And the instance of the 
annonymous class is created using the keyword new, so i am explicitly asking the JVM to create a new object in this case,
And creating a new object is not free. When i create a new object i need to get some memory. i need to
clean up that memory, then execute it, static initializers, then execute the static blocks, and then
the non-static initializers, and then the non-static blocks, and once all of this is done, i need to 
execute the contructor from the object class and from all the inheritance hierarchy from the object
class to the class i'm instancing. So all this process can be quite heavy. At least, it's an overhead, and i don't
have to pay this overhead when i'm creating a lambda expression. In fact, the JVM does not create a new object
every time i use a lambda expression.
N.B: So the answer is complex, it's not an object it is an object  without an identity :D

## Package java.util.function
### 4 catefories:
#### Supplier

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```
The first category is Supplier. It is just a single interface that does not take any object and that
provides a new object.
###Consumer
```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```
The second one is the reverse of the supplier, The consumer accepts an onject and doesn't return anything.
Now think on System.out.println(). this is the first example of a consumer one can think of.
```java
@FunctionalInterface
public interface BiConsumer<T, U> {
    void accept(T t, U u);
}
```
We also have BiConsumer. A biConsumer is a special kind of consumer that takes two objects instead of one.Now
those two objects don't have to be of the same type. they can be different.
### Predicate
```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```
It takes an object as a parameter and returns boolean. Also have BiPredicate like BeConsumer
```java
@FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
}
```
###Function
```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```
A function takes an object as a parameter and returns another object.Also BiFunction
```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
    R apply(T t, U u);
}
```
One Special cases of function is UnaryOperator that takes one object and return another object of
the same type.
```java
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T , T>{
}
```
## What is Method References??
It's just another way of writing a lambda expression, that it's strictly equivalent to ther other.
```java
Consumer<String> c = s-> System.out.println(S);
```
### Can be written like that
```java
Consumer<String> c = System.out::println;
```
It is composed of the object System.out and the name of the method that is invoked by this lambda 
expression, and both are separated by this new sign, (::)
### Another Example:
```java 
Comparator<Integer> c = (i1, i2) -> Integer.compare(i1, i2);
//Can be written like that
Comparator<Integer> c = Integer::compare;
```
## How Do We Process Data in Java?
The first question that we need to ask and to answer is, Where are our objects when we are in a java application?
Well the answer is quite simple. Most of the time, our objects are in an instance of collection. It's
might be a collection itself or be a List, a Set, the sorted set, the Map, but it still an instance
of the collection API or the collection Framework.Now, the second question is, can i process this data with lambdas?
What i would to do is the following.
List <Customer> list = ....;
    list.forEach(customer->System.out.println(customer));
And it will print all the customer.
    
## Default Methods
It allows to change the old interfaces without breaking the exiting implementations
Static methods are also allowed in java 8 interface!/
## Examples of new patterns
### Predicates
```java
Predicates<String> p1 = s-> s.lenght()< 20;
Predicates<String> p2 = s-> s.lenght()< 20;
Predicates<String> p3 = p1.and(p2);

@Functional interface
public interface Predicate<T>{

boolean test(T t);
default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }
}
```
So, let us see Examples Of New Patterns allowed with these two new concepts of default method and of static methods in interfaces. We saw the Predicate interface. It is part of the functional interfaces toolbox available in Java 8 in this Java utile function package, so let us take two of those predicates. The first one returns true. The string is of length less than 20, and the second one, greater than 10. We can change these two predicates with the boolean operation, and, and, what is this and stuff? Well, it is a method on the predicate interface, and it is a default method. When I write a predicate, when I implement a predicate, I do not need to provide an implementation of this and method, and, if I check the predicate interface, I can see the implementation of this and method. What does it take? It takes another predicate that should not be null. This is the first test inside the implementation, and, after that, what does it return? It just tests the variable with the first predicates and tests the variable with the second one, with the and boolean operation between those two tests. So, it is pretty simple and pretty straightforward to implement such methods. It has been done in the JDK. We have such default methods in all the functional interfaces of the toolbox we saw, and it is a good thing to just have a look at this code, just to take it as an example for us to get ideas on how to build new APIs with Java 8. We also have an isEqual method in predicates. What does it do? It returns a predicate that is a static method of the predicate's interface, and it returns a predicate that will just compare the given string to the target string that predicate is built on. Here is the implementation of that isEqual method. It takes the object as a target and just returns the result of target.equals the object passed as a parameter of the lambda expression.
## What is a Stream?
Stream is a java type interface. This type is T. It means that we can have streams of integer,
streams of person, streams of customer, streams of Strings etc etc. It is a brand new concept
and we could thing of a stream of collection. But in fact it is completely differnt from collection.
## What does it do?
It gives ways to efficiently process large amounts of data inside the JVM. It can process the data
in parralel and it also efficient to process small amount of data.
## What does efficiently mean?
First of all, this data can be processed in parallel, and I would say automatically processed in parallel. I don't have to write any kind of technical code to process my data in parallel. The parallelization of the algorithm doesn't have to be written by me as a developer. Why do I need to process my data in parallel? The answer is simple. It's just to leverage the computing power of multiple CPUs. Nowadays, all the CPU will use are multiple CPUs, and I want to leverage that. The second thing, all the process is conducted in a Pipeline, and this is extremely important to understand, too, because it will avoid unnecessary intermediary computations. Even if I have several operations on a given stream of data, all these operations are conducted on the one pass over the data. We are going to see that in more detail.
## Defination of java Stream
Why can't a Collection be a Stream? This is a very pertinent question, because, most of the time, my data is kept in an instance of a collection API, whether it is a list, a collection, or set, or whatever. So, why do I need this new concept to process my data? The answer might not be that simple. The answer is because stream is a new concept, and we need that new concept to process this data efficiently, and the people who designed the JDK didn't want to change the way the collection API works. We all know how the collection API works, and we certainly don't want to unlearn what we already know and learn new things about old stuff, as the collection API is. So, once again, what is a Stream and how can I build one? The stream is an object on which I can define operations, and by operations, you can think of a map, a filter, or a reduce operation. It's also an object that does not hold any data, and this is a big difference with the collection object. In a collection, I have my data, and I can do some basic operations on it. On a stream, I don't have any data. It's an object that should not change the data it processes. Why that? Just because I want to be able to process this data in parallel, that is, to distribute this data on the several CPUs I have or, at least, on the several calls of my CPU. So, since I don't want to be bothered by any visibility issue that would be solved with atomic variables, volatile variables, or synchronization, I decide that a stream is not allowed to change the data it processes. Now, if I build myself my own implementation of stream, and, if I decide to violate this rule and to modify the data I am processing, my code will still compile. The JVM will execute it, and I will run into big problems, exceptions, crashes, or corrupted data. So, this is just a rule. It is not enforced, not by the compiler, nor the JVM itself. And, it's also an object able to process data in one pass. What does it mean? Suppose I have a stream defined on a collection of person, the collection of person I used in the map/filter/reduce example, for instance, then I define three operations on it--the map, the filter, and the reduce. If I want to be efficient, I don't want to create any kind of inter- mediary collections to process this data, so it has to be processed in one pass. And, at last, this object should be optimized from the algorithm point of view and able to process data in parallel. What does it mean to be optimized from the algorithm point of view? It means that every time I declare an operation on it, the implementation of that operation, implementation that isn't a JDK, of course, should be optimal, as far as algorithm is concerned.
# A first Operation
```java
List<Persion> persons = .....;
Stream<Person> stream = persons.stream();
stream.forEach(p-> System.out.println(p));
```
How Can We Build a Stream? Well, in fact, we have many patterns to build streams. Let us see the first one, probably the most useful one. We have a stream method that has been added to the collection interface, so calling persons.stream will open a stream on the list of persons. What can we do with it? We can call, for instance, the forEach method defined on the stream interface and pass it a consumer. Here that consumer just prints out all the elements of the list. That forEach method takes an instance of Consumer as an argument. Consumer is a functional inter- face from the Java.util.function package, which is the toolbox that contains all the main functional interfaces provided in the JDK 8. Let us have a look at that Consumer interface.
```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```
It's a functional interface, so it has only one abstract method. This is the definition of a functional interface. It can be implemented by a lambda expression. Let's write Consumer c = p, this little arrow pointing to the right, System.out.println of p, and it can also be written as a method reference, System.out::println. In fact, Consumer is a bit more complex. I have a default method in the Consumer interface, as it is the case for most functional interface provided in the JDK.
```java
@Functional interface
public interface Consumer<T>{

void accept(T t);
default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}
```
 What does this default method do? It called andThen, and it will allow us to chain consumers. So, let us do that. Let us chain consumers. Let us create a list of string.
 ```java
 List<String> list = new ArrayList<>();
 List<Person> persons = ...;
 Consumer<String> c1 = result::add;
 Consumer<String> c2 = System.out::println;
 persons.stream()
                .forEach(c1.andThen(c2));
 ```
We define a first consumer. This Consumer takes a string s and adds it to our list, and the second Consumer, c2, just prints out the string s that is passed to it as a parameter. We can also write those to Consumer with method reference. Here is the result. And, we can just chain them, c3 = c1.andThen(c2). This will chain the Consumer c1 with the Consumer c2 by using the default method, andThen, defined on the Consumer interface. So, if I want to declare several consumers on the single stream, because the forEach method does not return anything, the only way I can do that is by chaining the consumers into one and passing them as a parameter to the forEach method, as it is the case on that example.
## A Second Operation is Filter operation.
```java
Predicate<String> p = Predicate.isEqual("two");
String<String> stream1 = Stream.of("one","two","three");
Stream<String> stream2 = stream1.filter(p);
```
## Intermediary and Terminal Operations
```java
public static void main(String args[]){
    Stream<String> stream = Stream.of("one","two","three","four","Five");
    Predicate<String> p1 = Predicate.isEqual("two");
    Predicate<String> p2 = Predicate.isEqual("three");
    
    List<String> list = new ArrayList<>();
    stream
            .peek(System.out::println)
            .filter(p1.or(p2))
            .peek(list::add)// later replace with .forEach(list::add);
            System.out.println("Done!");
            sout("size = "+list.size());
}
```
Now, let us take another example. Here we have the same stream of characters we had in the previous example, two predicates. The first one isEqual to two, and the second one isEqual to three, and we have an empty list that we are going to use in this example. So the aim of this example is to show the difference between intermediary operations and final operations. So what are we going to do? We take our stream, and we are going to peek it with the System.out::println operation. Then, filter with p1.or(p2), same case as previously, and then peek again, but this time we will add the filter content to our list, so list::add. Since the peek method returns a stream and the filter method returns also a stream, both are intermediary operations, so if I execute that code, nothing should happen, and the list should remain empty. So, we can verify that. Let us add a message at the end of this execution, and let us print out the size of the list, list.size. Let us execute that. We should see the Done message and size equals to zero. This is the case. This piece of code System.out::println hasn't been executed. The size is still zero. It means that list.add hasn't been executed either. Now, if we replace the peek call by a forEach call, what is the difference between the peek and the forEach? The forEach doesn't return anything, so it is not an intermediary operation. It is a final operation. This time, this final operation should trigger the processing of the data that the stream is connected to in one pass over the data. So, I should see the execution of this System.out.println. It should print one, two, three, four, five. Why? Because it is executed before the filter step, then the filter step will occur. We only keep the two and the three strings from that stream, and then, the forEach will add the content to that stream. Let us execute that. Okay, what do we have? One, two, three, four, five. This is the execution of the System.out.println here. Then we have the Done, which is printed. It is correct, and then, the size of the list, which is two, meaning that the two and three strings have been correctly added in the forEach step. This explains and this shows the difference between intermediary and final operation, and, keep in mind, only a final operation will trigger the processing of the data the stream is connected to.
