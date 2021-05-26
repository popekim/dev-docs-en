---
title: "Java Coding Standards"
date: 2021-05-25
---

## Preface

### Rule of Thumb
1. Readability first (your code should be your documentation most of the time)
2. Follow IDE's auto-formatted style unless you have really good reasons not to do so. (Ctrl + Alt + L in IntelliJ on Windows)
3. Learn from existing code

## References
This coding standards is inspired by these coding standards

* [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
* [Android Open Source Project Java Code Style for Contributors](https://source.android.com/setup/contribute/code-style)
* [Java Code Conventions](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf) by Oracle


## I. Main Coding Standards
1. Use all lower case for package name

    ```java
    package com.awesome.math;
    ```

2. Always use fully qualified imports (No wildcard imports)
    
    BAD:
    ```java
    import com.awesome.*;
    ```

    GOOD:
    ```java
    import foo.bar;
    ```
3. Use Pascal casing for classes and enums

    ```java
    public class PlayerManager {
        // some code
    }
    ```

    ```java
    public enum AccountType {
        // some enums
    }
    ```

4. Always add access modifiers to classes, member variables and methods EXCEPT when default (package) access is needed

    ```java
    public class Person {
        int mHeight; // this is default (package) access
        private int age;

        public int getAge() {
            // method implementation...
        }

        private void doSomething() {
            // method implementation...
        }
    }
    ```

5. Put access modifiers before non-access modifiers

    BAD:
    ```java
    static public void doSomething() {
        // method implementation
    }
    ```

    GOOD:
    ```java
    public static void doSomething() {
        // method implementation
    }
    ```

6. Use camel casing for all method names

    ```java
    public int getAge() {
        // method implementation...
    }
    ```

7. Use camel casing for local variable names and method parameters

    ```java
    int age = 10;

    public void someMethod(int someParameter) {
        int someNumber;
    }
    ```

8. Use verb-object pairs for method names

    ```java
    public int getAge() {
        // method implementation...
    }
    ```

9. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for `final` variables (constants)

    ```java
    final int SOME_CONSTANT = 1;
    ```

10. Prefix interfaces with `I`

    ```java
    interface IFlyable;
    ```

11. Enum members are ALL_CAPS_SEPARATED_BY_UNDERSCORE

    ```java
    public enum MyEnum {
        FUN,
        MY_AWESOME_VALUE
    }
    ```

12. Use camel casing for member variables

    ```java
    public class Employee {
        public String nickName;
        protected String familyName;
        private int age;
    }
    ```

13. Methods with return values must have a name describing the value returned

    ```java
    public int getAge();
    ```

14. Use descriptive variable names. e.g `index` or `employee` instead of `i` or `e` unless it is a trivial index variable used for a loop.

    ```java
    int i; // BAD
    int a; // BAD
    int index; // GOOD
    int age; // GOOD
    ```
    ```java    
    // GOOD
    for (int i = 0; i < 10; ++i) {

    }
    ```

15. Treat acronyms just like any other words. If using PascalCasing, capitalize only the first letter. If using camelCasing, capitalize the first letter only if there is another word before it.

    ```java
    int orderId
    String httpAddress;
    String myHttp;
    ```

16. Use getter and setter methods over `public` member variables

    BAD:
    ```java
    public class Employee {
        public String Name;
    }
    ```

    GOOD:
    ```java
    public class Employee {
        private String name;

        public String getName();
        public String setName(String name);
    }
    ```


17. Declare local variables as close as possible to the first line where it is being used.


18. Use precision specification for floating point values unless there is an explicit need for a `double`.

    ```java
    float f = 0.5f;
    ```

19. Always have a `default`: case for a `switch` statement.

    ```java
    switch (number) {
        case 0:
            ... 
            break;
        default:
            break;
    }
    ```

20. When intentionally falling through to the next `case`, add `// intentional fallthrough` comment.

    ```java
    switch (number) {
        case 0:
            doSomething();
            // intentional fallthrough
        case 1:
            doFallthrough();
            break;
        case 2:
        case 3:
            doNotFallthrough();
            break;
        default:
            break;
    }
    ```

21. If `default`: case must not happen in a `switch` case, always add `assert (false);` or throw an exception.

    ```java
    switch (type) {
        case 1:
            ... 
            break;
        default:
            assert (false) : "unknown type";
            break;
    }
    ```

    OR

    ```java
    switch (type) {
        case 1:
            ... 
            break;
        default:
            throw new IllegalArgumentException("unknown type");
            break;
    }
    ```

22. Names of recursive methods end with `Recursive`.

    ```java
    public void fibonacciRecursive();
    ```


23. Order of class variables and methods must be as follows:

    ```
    a. public member variables
    b. default member variables
    c. protected member variables
    d. private member variables
    e. constructors
    f. public methods
    g. default methods
    h. protected methods
    i. private methods
    ```


24. Method overloading must be avoided in most cases

    Use:
    ```java
    public Anim getAnimByIndex(int index);
    public Anim getAnimByName(String name);
    ```

    Instead of:
    ```java
    public Anim getAnim(int index);
    public Anim getAnim(String name);
    ```

25. Each class must be in a separate source file unless it makes sense to group several smaller classes.


26. The filename must be the same as the name of the top-level class including upper and lower cases.

    ```java
    // File: PlayerAnimation.java

    public class PlayerAnimation {

        private class InnerClass1 {

        }
        
        private class InnerClass2 {

        }
    }
    ```


27. Use `assert` for any assertion you have. Assert is not recoverable. (e.g, most method will have `assert (parameter != null)`)

28. Put brackets around the expression when using `assert`.

    BAD:
    ```java
    assert x > 5 && x < 0 : "Custom message";
    ```

    GOOD:
    ```java
    assert (x > 5 && x < 0) : "Custom message"
    ```

29. The name of a bitflag enum must be suffixed by `Flags`

    ```java
    public enum VisibilityFlags {
        // some flags
    }
    ```

30. Shadowed variables are not allowed. The only exception of this rule is when the same name is used for a member variable and a constructor/setter parameter.

    ```java
    public class SomeClass {
        private int count = 5;

        public void func() {
            for (int count = 0; count != 10; ++count) {
                System.out.println(count);
                System.out.println(this.count);
            }
        }
    }
    ```

31. Prefer to use real type over implicit typing(i.e, `var`) unless the type is obvious from the right side of assignment, or when the type is unimportant. Some acceptable var usage includes `IIterable`/`ICollection` and when `new` keyword is on the same line, showing what type of object is being created clearly.

    ```java
    var text = "string obviously"; // Okay
    var age = 28;                  // Okay
    var employee = new Employee(); // Okay

    var accountNumber1 = getAccountNumber(); // BAD
    int accountNumber2 = getAccountNumber(); // Good
    ```

32. Validate any external data at the boundary and return before passing the data into our methods. This means that we assume all data is valid after this point.

33. Therefore, try not to throw any exception from non-boundary methods. Also exceptions should be handled at the boundary only.


34. As an exception to the previous rule, exception throwing is allowed when `switch`-`default` is used to catch missing `enum` handling logic. Still, do not catch this exception

    ```java
    switch (accountType) {
        case AccountType.PERSONAL:
            return something;
        case AccountType.BUSINESS:
            return somethingElse;
        default:
            throw new IllegalArgumentException("unknown type");
    }
    ```

35. Prefer not to allow `null` parameters in your method, especially from a `public` one.


36. If `null` parameter is used, postfix the parameter name with `OrNull`

    ```java
    public Anim getAnim(string nameOrNull) {
        ...
    }
    ```


37. Prefer not to return `null` from any method, especially from a `public` one. However, you sometimes need to do this to avoid throwing exceptions.

38. If `null` is returned from any method, postfix the method name with `OrNull`.

    ```java
    public String getNameOrNull();
    ```

39. Always use `@Override` annotation when overriding methods.


## II. Code Formatting
1. Use IntelliJ's default for tabs. If another IDE is used, use 4 spaces instead of a real tab.

2. Put opening curly brace (`{`) on the same line as the code before it, not on their own line, while closing brace(`}`) must be on its own line, EXCEPT (see below)

    ```java
    public void myMethod() {
        while (expression) {
            // code here
        }

        try {
            // code here
        } catch (ExceptionClass ex) {
            // code here
        }
    }
    ```

3. Use the following form for `if`, `if-else`, `if-else-if-else` statement

    ```java
    if (expression) {
        // code here
    } else if (expression){
        // code here
    } else {
        // code here
    }
    ```

4. ALWAYS add curly braces even if there's only one line in the scope

    ```java
    if (!alive) {
        return;
    }
    ```

5. Declare only one variable per line

    BAD:
    ```java
    int counter = 0, index = 0;
    ```
    
    GOOD:
    ```java
    int counter = 0;
    int index = 0;
    ```