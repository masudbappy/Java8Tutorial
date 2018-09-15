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
So, let us see Examples Of New Patterns allowed with these two new concepts of default method and of static methods in interfaces. We saw the Predicate interface. It is part of the functional interfaces toolbox available in Java 8 in this Java utile function package, so let us take two of those predicates. The first one returns true. The string is of length less than 20, and the second one, greater than 10. We can change these two predicates with the boolean operation, and, and, what is this and stuff? Well, it is a method on the predicate interface, and it is a default method. When I write a predicate, when I implement a predicate, I do not need to provide an implementation of this and method, and, if I check the predicate interface, I can see the implementation of this and method. What does it take? It takes another predicate that should not be null. This is the first test inside the implementation, and, after that, what does it return? It just tests the variable with the first predicates and tests the variable with the second one, with the and boolean operation between those two tests. So, it is pretty simple and pretty straightforward to implement such methods. It has been done in the JVK. We have such default methods in all the functional interfaces of the toolbox we saw, and it is a good thing to just have a look at this code, just to take it as an example for us to get ideas on how to build new APIs with Java 8. We also have an isEqual method in predicates. What does it do? It returns a predicate that is a static method of the predicate's interface, and it returns a predicate that will just compare the given string to the target string that predicate is built on. Here is the implementation of that isEqual method. It takes the object as a target and just returns the result of target.equals the object passed as a parameter of the lambda expression.

