# 6. Arrays
## Arrays intro
You can have arrays of any primitive type, or arrays of any class. An array is not resizable. 

"dev/lpa/Main.java":
```java
package dev.lpa;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] myIntArray = new int[10]; // can also be: int myIntArray[] = ...
        myIntArray[0] = 45;
        myIntArray[1] = 1;
        myIntArray[5] = 50;

        double[] myDoubleArray = new double[10];
        myDoubleArray[2] = 3.5;
        System.out.println(myDoubleArray[2]);

        int[] firstTen = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}; // array initializer.
        // same as: int[] firstTen = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}; "
        System.out.println("first = " + firstTen[0]);
        int arrayLength = firstTen.length;
        System.out.println("length of array = " + arrayLength);
        System.out.println("last = " + firstTen[arrayLength - 1]);

        int[] myArray;
        myArray = new int[] {5, 4, 3, 2, 1}; // array initializer, cannot omit "int[]" here
        for (int i = 0; i < myArray.length; i++) {
            System.out.print(myArray[i] + " ");
        }

        int[] newArray;
        newArray = new int[5];
        for (int i = 0; i < newArray.length; i++) { // use loop to assign vals
            newArray[i] = newArray.length - i;
        }
        for (int i = 0; i < newArray.length; i++) {
            System.out.print(newArray[i] + " ");
        }
        System.out.println();
        for (int elem : newArray) { // use the enhanced for loop
            System.out.print(elem + " ");
        }
        System.out.println();
        System.out.println(newArray); // prints the hashcode
        System.out.println(Arrays.toString(newArray)); // [5, 4, 3, 2, 1]

        Object objectVariable = newArray; // assign an array to an object variable
        if (objectVariable instanceof int[]) {
            System.out.println("objectVariable is really an int array");
        }

        Object[] objectArray = new Object[3]; // an array of 3 objects
        objectArray[0] = "Hello";
        objectArray[1] = new StringBuilder("World");
        objectArray[2] = newArray;
    }
}

```

Note that you cannot use the anonymous version of the array initializer in a statement outside of declaration statement. 

An array is a special class in Java. It is still a class, and ultimately inherits from java.lang.Object. 

When you do not initialize an array, all its elems get initialized to the default val for that datatype. For primitive vals, it is 0 for numeric type; for booleans, it is false; for any class type, it is null. 

## java.util.Arrays
"dev/lpa/Main.java":
```java
package dev.lpa;
import java.util.Arrays;
import java.util.Random;

public class Main {
    public static void main(String[] args) {
        int[] firstArray = getRandomArray(10);
        System.out.println(Arrays.toString(firstArray));
        Arrays.sort(firstArray); // sort the array in place
        System.out.println(Arrays.toString(firstArray));

        int[] secondArray = new int[10];
        System.out.println(Arrays.toString(secondArray)); // return all 0s
        Arrays.fill(secondArray, 5); // fill the array with 5s
        System.out.println(Arrays.toString(secondArray));

        int[] thirdArray = getRandomArray(10);
        System.out.println(Arrays.toString(thirdArray));

        int[] fourthArray = Arrays.copyOf(thirdArray, thirdArray.length); // this creates a new array, and copy the values over to it.
        System.out.println(Arrays.toString(fourthArray));

        Arrays.sort(fourthArray);
        System.out.println(Arrays.toString(thirdArray)); // the old array is not affected by sorting
        System.out.println(Arrays.toString(fourthArray));

        int[] smallerArray = Arrays.copyOf(thirdArray, 5); // copy over the 1st 5 elems
        System.out.println(Arrays.toString(smallerArray));

        int[] largerArray = Arrays.copyOf(thirdArray, 15); // fill 0s if 15 exceed original array length
        System.out.println(Arrays.toString(largerArray));

        String[] sArray = {"Able", "Jane", "Mark", "Ralph", "David"};
        Arrays.sort(sArray);
        System.out.println(Arrays.toString(sArray));
        if (Arrays.binarySearch(sArray, "Mark") >= 0) {
            System.out.println("Found Mark in the list");
        }

        int[] s1 = {1, 2, 3, 4, 5};
        int[] s2 = {1, 2, 3, 4, 5, 0};
        if (Arrays.equals(s1, s2)) { // compare the order, and the values
            System.out.println("Arrays are equal");
        } else {
            System.out.println("Arrays are not equal");
        }
    }

    private static int[] getRandomArray(int len) {
        Random random = new Random();
        int[] myArray = new int[len];
        for (int i = 0; i < len; i++) {
            myArray[i] = random.nextInt(100);
        }
        return myArray;
    }
}
```

## References Types vs Value Types
"dev/lpa/Main.java":
```java
package dev.lpa;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] myIntArray = new int[5];
        int[] anotherArray = myIntArray;

        System.out.println("myIntArray = " + Arrays.toString(myIntArray));
        System.out.println("anotherArray = " + Arrays.toString(anotherArray));

        anotherArray[0] = 1;
        modifyArray(myIntArray);

        System.out.println("after change myIntArray = "
                + Arrays.toString(myIntArray));// 1, 2, 0, 0, 0
        System.out.println("after change anotherArray = " +
                Arrays.toString(anotherArray));// 1, 2, 0, 0, 0
    }

    private static void modifyArray(int[] array) {
        array[1] = 2;
    }
}
```

## Variable arguments
"dev/lpa/Main.java": 
```java
package dev.lpa;

public class Main {
    public static void main(String... args) { // means takes 0/1/many strings
        String[] splitStrings = "Hello World again".split(" ");
        printText(splitStrings); // passes an array of strs

        printText("Hello"); // passes a single str
        printText("Hello", "World", "again"); // passes 3 strs
        printText(); // passes nothing

        // example of Java own classes that uses variable argument
        String[] sArray = {"first", "second", "third", "fourth", "fifth"};
        System.out.println(String.join(",", sArray));
    }

    private static void printText(String... textList) {
        for (String t : textList) {
            System.out.println(t);
        }
    }
}
```
`String... args` is the "variable argument" parameter. It can take a string array, or a single string, many strings, or nothing. 

Rules: There can only be one variable argument in a method, and it must be the last argument. 

Java uses variable arguments in many methods in their library classes. For example, the Array.join() has its last argument as a variable argument, that is why the delimiter needs to be specified first in the argument list. 

## Two-dimensional and multi-dimensional arrays
To declare a 2d array literal: `int[][] array = {{1, 2}, {4, 5, 6}, {10}};`. Note that 2d array not need to have a uniform matrix. 

"dev/lpa/Main.java": 
```java
package dev.lpa;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[][] arr = new int[4][4]; // default all vals to 0s
        System.out.println(Arrays.toString(arr)); // print out 4 hashes
        System.out.println("arr.length = " + arr.length);

        for (int[] outer : arr) {
            System.out.println(Arrays.toString(outer)); // print out 4 arrays of 4 0s
        }

        for (int i = 0; i < arr.length; i++) {
            var innerArray = arr[i];
            for (int j = 0; j < innerArray.length; j++) {
                arr[i][j] = (i * 10) + (j + 1);
            }
        }
        System.out.println(Arrays.deepToString(arr)); // print multi-dim arr

        arr[1] = new int[] {10, 20, 30};
        System.out.println(Arrays.deepToString(arr)); // not restricted to fix size

        Object[] anyArray = new Object[3];
        System.out.println(Arrays.toString(anyArray));

        anyArray[0] = new String[] {"a", "b", "c"};
        anyArray[1] = new String[][]{
                {"1", "2"},
                {"3", "4", "5"},
                {"6", "7", "8", "9"}
        };
        anyArray[2] = new int[2][2][2];
        // anyArray[2] = "Hello"; // will have error in line x, because it is not an object array
        System.out.println(Arrays.deepToString(anyArray));

        for (Object element : anyArray) {
            System.out.println("Element type = " + element.getClass().getSimpleName());
            System.out.println("Element toString() = " + element);
            System.out.println(Arrays.deepToString((Object[]) element)); // line x
        }
    }
}
```

It is a best practice, to stick to more strictly typed arrays. 
