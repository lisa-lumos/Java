# Getting started
The current LTS version is Java 17, and Oracle (the owner of Java) will support it till at least 2029.

## Installing Java 17
Go to `https://java.sun.com/`, and click on the hyperlink `Java SE 17.X.X (LTS)`, download and install according to OS. 

To check the installation, open a terminal, and type `java -version`, and should get below output: 
```console
lisa@mac16 ~> java -version
java version "17.0.5" 2022-10-18 LTS
Java(TM) SE Runtime Environment (build 17.0.5+9-LTS-191)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.5+9-LTS-191, mixed mode, sharing)
```

## Intro to JShell
Using an integrated development environment (IDE) is the most commonly used method of writing Java code. 

JShell: Read-Eval-Print-Loop (REPL) interactive program, a playground/sandbox that runs in a terminal, and is useful for quickly trying out new ideas:
- reads the command or code segment we type in
- evaluates and executes the code, and allows shortcuts to be used
- prints out the results of the evaluation, without making the developer write code to output the results
- loops right back for more input (code segments or commands)

JShell is not an IDE, it is just a handy way to quickly get started with Java. 

Java Shell user guide: `https://docs.oracle.com/en/java/javase/19/jshell/preface.html#GUID-E69066DF-2615-48B4-BD34-679C0FB67C33`. 

When you want to write a couple of lines in JShell, you can wrap them in curly braces. 

```console
lisa@mac16 ~> jshell                                                     (base) 
|  Welcome to JShell -- Version 17.0.5
|  For an introduction type: /help intro

jshell> /help intro
|  
|                                   intro
|                                   =====
|  
|  The jshell tool allows you to execute Java code, getting immediate results.
|  You can enter a Java definition (variable, method, class, etc), like:  int x = 8
|  or a Java expression, like:  x + x
|  or a Java statement or import.
|  These little chunks of Java code are called 'snippets'.
|  
|  There are also the jshell tool commands that allow you to understand and
|  control what you are doing, like:  /list
|  
|  For a list of commands: /help

jshell> /help
|  Type a Java language expression, statement, or declaration.
|  Or type one of the following commands:
|  /list [<name or id>|-all|-start]
|  	list the source you have typed
|  /edit <name or id>
|  	edit a source entry
|  /drop <name or id>
|  	delete a source entry
|  /save [-all|-history|-start] <file>
|  	Save snippet source to a file
|  /open <file>
|  	open a file as source input
|  /vars [<name or id>|-all|-start]
|  	list the declared variables and their values
|  /methods [<name or id>|-all|-start]
|  	list the declared methods and their signatures
|  /types [<name or id>|-all|-start]
|  	list the type declarations
|  /imports 
|  	list the imported items
|  /exit [<integer-expression-snippet>]
|  	exit the jshell tool
|  /env [-class-path <path>] [-module-path <path>] [-add-modules <modules>] ...
|  	view or change the evaluation context
|  /reset [-class-path <path>] [-module-path <path>] [-add-modules <modules>]...
|  	reset the jshell tool
|  /reload [-restore] [-quiet] [-class-path <path>] [-module-path <path>]...
|  	reset and replay relevant history -- current or previous (-restore)
|  /history [-all]
|  	history of what you have typed
|  /help [<command>|<subject>]
|  	get information about using the jshell tool
|  /set editor|start|feedback|mode|prompt|truncation|format ...
|  	set configuration information
|  /? [<command>|<subject>]
|  	get information about using the jshell tool
|  /! 
|  	rerun last snippet -- see /help rerun
|  /<id> 
|  	rerun snippets by ID or ID range -- see /help rerun
|  /-<n> 
|  	rerun n-th previous snippet -- see /help rerun
|  
|  For more information type '/help' followed by the name of a
|  command or a subject.
|  For example '/help /list' or '/help intro'.
|  
|  Subjects:
|  
|  intro
|  	an introduction to the jshell tool
|  keys
|  	a description of readline-like input editing
|  id
|  	a description of snippet IDs and how use them
|  shortcuts
|  	a description of keystrokes for snippet and command completion,
|  	information access, and automatic code generation
|  context
|  	a description of the evaluation context options for /env /reload and /reset
|  rerun
|  	a description of ways to re-evaluate previously entered snippets

jshell> /list -all

  s1 : import java.io.*;
  s2 : import java.math.*;
  s3 : import java.net.*;
  s4 : import java.nio.file.*;
  s5 : import java.util.*;
  s6 : import java.util.concurrent.*;
  s7 : import java.util.function.*;
  s8 : import java.util.prefs.*;
  s9 : import java.util.regex.*;
 s10 : import java.util.stream.*;

jshell> /list -start

  s1 : import java.io.*;
  s2 : import java.math.*;
  s3 : import java.net.*;
  s4 : import java.nio.file.*;
  s5 : import java.util.*;
  s6 : import java.util.concurrent.*;
  s7 : import java.util.function.*;
  s8 : import java.util.prefs.*;
  s9 : import java.util.regex.*;
 s10 : import java.util.stream.*;

jshell> {
   ...>     
   ...>     
   ...> }

jshell> /exit
|  Goodbye
lisa@mac16 ~> 
```



