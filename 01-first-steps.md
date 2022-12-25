# 1. First steps
## Hello World
To print out Hello World in JShell:
```console
jshell> System.out.print("Hello World"); // string literal
Hello World
jshell> System.out.print("Hello Round!");
Hello Round!
```

## Variables
Java is case sensitive. 
```console
jshell> int myNumber = 5;
myNumber ==> 5

jshell> System.out.print(myNumber);
5
jshell> myNumber = 10;
myNumber ==> 10

jshell> System.out.print(myNumber);
10
jshell> /list

   1 : System.out.print("Hello World");
   2 : System.out.print("Hello Round!");
   3 : int myNumber = 5;
   4 : System.out.print(myNumber);
   5 : myNumber = 5;
   6 : myNumber = 10;
   7 : System.out.print(myNumber);

jshell> myNumber = 10 + 5;
myNumber ==> 15

jshell> myNumber = (10 + 5) + 2 * 10
myNumber ==> 35
```

Note that JShell allows you to redeclare a variable, but in a normal java code block, you are not allowed to do that. 

```
jshell> int my1stNum = (10 + 5) + 2 * 10;
my1stNum ==> 35

jshell> int my2ndNum = 12;
my2ndNum ==> 12

jshell> int my3rdNum = 6;
my3rdNum ==> 6

jshell> /var
|    int my1stNum = 35
|    int my2ndNum = 12
|    int my3rdNum = 6

jshell> int myTotal = my1stNum + my2ndNum + my3rdNum;
myTotal ==> 53
```
Note that `/var` shows you the vars created in current session. 


















