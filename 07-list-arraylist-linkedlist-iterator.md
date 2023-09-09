# List, ArrayList, LinkedList, Iterator, Autoboxing
Items in Arrays all have the same type. You cannot change the number of elements in an Array. 

Java has a library for containers(Collections), which have many improvements over the Array. 

## List and ArrayList
List is a Java Interface, which describes a set of method signatures, that all classes that implement this interface are expected to have. 

ArrayList implements the List interface, and is a resizable array. 

"src/dev/lpa/Main.java":
```java
package dev.lpa;

import java.util.ArrayList;
import java.util.Arrays;

record GroceryItem(String name, String type, int count) {
    public GroceryItem(String name) {
        this(name, "DAIRY", 1);
    }
}

public class Main {
    public static void main(String[] args) {
        // declaring arrays with a specific type, allows compile-time type checking
        // which prevents run-time exceptions
        GroceryItem[] groceryArray = new GroceryItem[3];
        groceryArray[0] = new GroceryItem("milk");
        groceryArray[1] = new GroceryItem("apples", "PRODUCE", 6);
        groceryArray[2] = new GroceryItem("oranges", "PRODUCE", 5);
        System.out.println(Arrays.toString(groceryArray));

        // if arrayList doesn't have a type, it will use Object as default
        // called the "raw use of this type"
        ArrayList objectList = new ArrayList(); 
        objectList.add(new GroceryItem("Butter"));
        objectList.add("Yogurt"); // takes any objects

        // the type in the angled brackets is the type of elements
        ArrayList<GroceryItem> groceryList = new ArrayList<>();
        groceryList.add(new GroceryItem("Butter"));
    }
}

```


## LinkedList



## Iterators




## Autoboxing and Unboxing




## The enum Type





























