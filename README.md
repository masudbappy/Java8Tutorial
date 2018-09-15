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
