# 2. IntelliJ Basics
An IDE is the easiest, least error-prone way to develop, manage and deploy Java classes. 

## IntelliJ Settings
- Appearance & Behavior -> Appearance -> Theme: light
- Editor -> General -> Auto Import -> Check "Add unambiguous imports on the fly" and "Optimize imports on the fly"
- Editor -> General -> Code Folding -> uncheck "File header" and "Imports" under General pane, "One-line methods", "Closures", "Generic constructor and method parameters" under Java pane
- Editor -> General -> Appearance -> check "Show line numbers"

## Hello World
Create a New Project
- Name: HelloWorld (Note: recommend upper camel casing in project names)
- Location: ~/Desktop/java/java-projects
- Language: Java
- Build system: IntelliJ
- JDK: Oracle OpenJDK version 17.0.5
- Uncheck "Add sample code"

The folder .idea contains IntelliJ's working files, we ignore it for now. Right-click on the src folder -> New -> Java Class. Name it FirstClass, and press Enter. Note the the file FirstClass.java file opens in the editor, and in the left pane, its icon has a c inside, which means it is a class. 

Inside this class, type "psvm" and hit Enter. This will create a main method. Print hello world inside:
```java
public class FirstClass { // "public" keyword is access modifier, it is optional
    public static void main(String[] args) { // the main method
        System.out.println("Hello World! ");
    }
}
```

Three ways to run a code:
1. click the run icon in the top bar
2. click the green run icon after the line number
3. right click the editor and click Run 'FirstClass.main()'

The output is:
```console
/Library/Java/JavaVirtualMachines/jdk-17.0.5.jdk/Contents/Home/bin/java -javaagent:/Applications/IntelliJ IDEA CE.app/Contents/lib/idea_rt.jar=53077:/Applications/IntelliJ IDEA CE.app/Contents/bin -Dfile.encoding=UTF-8 -classpath /Users/lisa/Desktop/java/java-projects/HelloWorld/out/production/HelloWorld FirstClass
Hello World!
 
Process finished with exit code 0
```

## Ternary/Conditional operator
`operand1 ? operand2 : operand3` - if operand1 evaluates to true, then return operand2, otherwise return operand3. It is a shortcut of the if-then-else statement. 

```java
String makeOfCar = "Ford";
String conclusion = (makeOfCar.equals("Ford")) ? "is domestic" : "is not domestic"; // parenthesis makes it more readable
System.out.println(conclusion); // "is domestic"
```

In IntelliJ, double click a editor tab expand/collapse the File Explorer. 








