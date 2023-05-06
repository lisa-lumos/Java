# 3. Expressions, Statements, Code Blocks, Methods
Refer to the Google Java Style Guide for coding conventions. 

Java method: a method declares executable code that can be invoked, passing a fixed num of vals as arguments. 

In a prod env, it is critical to be aware of the effect your changes to a method will have, as there can be code dependent on it. 

The main method is special in Java, because Java's virtual machine (JVM) looks for this method and uses it as the entry point for execution of code. 

A java method example:
```java
public class MainChallenge {

    public static void main(String[] args) {

        boolean gameOver = true;
        int score = 800;
        int levelCompleted = 5;
        int bonus = 100;

        int highScore = calculateScore(gameOver, score, levelCompleted, bonus);
        System.out.println("The highScore is " + highScore);

        score = 10000;
        levelCompleted = 8;
        bonus = 200;

        System.out.println("The next highScore is " +
                calculateScore(gameOver, score, levelCompleted, bonus));
    }

    public static int calculateScore(boolean gameOver, int score, int levelCompleted, int bonus) {

        int finalScore = score;

        if (gameOver) {
            finalScore += (levelCompleted * bonus);
            finalScore += 1000;
        }

        return finalScore;
    }
}
```

Java doesn't support default values for parameters in a method. 

In IntelliJ, you can `compare two files` by right clicking a file in the Project folder, then select "Compare With...". With two class files side by side, you can also sync differences of two file by clicking the arrows at each difference. 

You can also see `code history in the IntelliJ`, by right-click the file in the Project folder, and click "Local History". This again compares an older version you select, and allow you to compare side by side, and you can revert by clicking the arrows near the line numbers. You can also revert to an old version in one click by right click on that version, then "Revert". 

## Method overloading
When a class has many methods with the same name, but with different signatures (num & order & datatype of params). Return type is not included. 
```java
public class Main {

    public static void main(String[] args) {

        System.out.println("New score is " + calculateScore("Tim", 500));
        System.out.println("New score is " + calculateScore(10));
    }

    public static int calculateScore(String playerName, int score) {

        System.out.println("Player " + playerName + " scored " + score + " points");
        return score * 1000;
    }

    public static int calculateScore(int score) {

        return calculateScore("Anonymous", score);
    }

    public static int calculateScore() {

        System.out.println("No player name, no player score.");
        return 0;
    }
}

```

















