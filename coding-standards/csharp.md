---
title: "C# Coding Standards"
date: 2021-05-25
---

## Preface

### Rule of Thumb

1. Readability first (your code should be your documentation most of the time)
2. Follow IDE's auto formatted style unless you have really good reasons not to do so. (Ctrl + K + D in Visual Studio)
3. Learn from existing code


### References

This coding standards is inspired by these coding standards

* [Unreal engine 4 coding standard](https://docs.unrealengine.com/latest/INT/Programming/Development/CodingStandard/)
* [Doom 3 Code Style Conventions](ftp://ftp.idsoftware.com/idstuff/doom3/source/codestyleconventions.doc)
* [IDesign C# Coding Standard](http://www.idesign.net/downloads/getdownload/1985)

### IDE Helper

The settings that can imported into your IDE can be found [here](https://github.com/popekim/CodingStyle/tree/master/CSharp).



## I. Main Coding Standards

1. Use Pascal casing for class and structs

    ```cs
    class PlayerManager;
    struct PlayerData;
    ```

2. Use camel casing for local variable names and function parameters

   ```cs
   public void SomeMethod(int someParameter)
   {
       int someNumber;
       int id;
   }
   ```


3. Use verb(base form)-object pairs for method names, by default.

    ```cs
    public uint GetAge()
    {
        // function implementation...
    }
    ```


4. However, if a method simply returns a boolean state, the verb part of the name should be prefixed Is, Can, Has or Should. If the function name becomes not natural by doing so, use the 3rd-person singular form of another verb.

    ```cs
    public bool IsAlive(Person person);
    public bool Has(Person person);
    public bool CanAccept(Person person);
    public bool ShouldDelete(Person person);
    public bool Exists(Person person);
    ```

5. Use pascal casing for all method names except (see below)

    ```cs
    public uint GetAge()
    {
        // function implementation...
    }
    ```

6. Use camel case for any non-public method. You might need to add custom Visual Studio style rule as described here

    ```cs
    private uint getAge()
    {
        // function implementation...
    }
    ```

7. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for constants

    ```cs
    const int SOME_CONSTANT = 1;
    ```

8. Use static readonly if an object is a constant

   ```cs
   public static readonly MyConstClass MY_CONST_OBJECT = new MyConstClass();
   ```

9. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for `static readonly` variables

10. Use `readonly` when a variable must be assigned only once

    ```cs
    public class Account
    {
        private readonly string mPassword;
    
        public Account(string password)
        {
            mPassword = password;
        }
    }
    ```

11. Use pascal casing for namespaces
    
    ```cs
    namespace System.Graphics
    ```

12. prefix boolean variables with `b`.

    ```cs
    bool bFired;		// for local variable
    private bool mbFired;	// for private member variable
    ```

13. prefix boolean properties with `Is`, `Can`, `Should` or `Has`. 

    ```cs
    public bool IsFired { get; private set; }
    public bool HasChild { get; private set; }
    public bool CanModal { get; private set; }
    public bool ShouldRedirect { get; private set; }
    ```


14. prefix interfaces with `I`

    ```cs
    interface ISomeInterface;
    ```

15. prefix enums with `E`

    ```cs
    public enum EDirection
    {
        North,
        South
    }
    ```

16. prefix structs with `S`

    ```cs
    public struct SUserID;
    ```

17. prefix `private` member variables with `m`. Use Pascal casing for the rest of a member variable
    
    ```cs
    Public class Employee
    {
        public int DepartmentID { get; set; }
        private int mAge;
    }
    ```

18. Methods with return values must have a name describing the value returned

    ```cs
    public uint GetAge();
    ```

19. Use descriptive variable names. e.g `index` or `employee` instead of `i` or `e` unless it is a trivial index variable used for loops.

20. Capitalize every character in acronyms only if there is no extra word after them.

    ```cs
    public int OrderID { get; private set; }
    public string HttpAddress { get; private set; }
    ```

21. Prefer properties over getter setter functions


    BAD:
    ```cs
    public class Employee
    {
        private string mName;
        public string GetName();
        public string SetName(string name);
    }
    ```

    GOOD:
    ```cs
    public class Employee
    {
        public string Name { get; set; }
    }
    ```

22. Declare local variables as close as possible to the first line where it is being used.


23. Use precision specification for floating point values unless there is an explicit need for a `double`.

    ```cs
    float f = 0.5F;
    ```

24. Always have a `default`: case for a `switch` statement.

    ```cs
    switch (number)
    {
        case 0:
            ... 
            break;
        default:
            break;
    ```


25. If `default`: case must not happen in a `switch` case, always add `Debug.Assert(false);` or `Debug.Fail();`

    ```cs
    switch (type)
    {
        case 1:
            ... 
            break;
        default:
            Debug.Fail("unknown type");
            break;
    }
    ```

26. Names of recursive functions end with `Recursive`

    ```cs
    public void FibonacciRecursive();
    ```

27. Order of class variables and methods must be as follows:

    ```
    a. public variables/properties
    b. internal variables/properties
    c. protected variables/properties
    d. private variables
    Exception: if a private variable is accessed by a property, it should appear right before the mapped property.
    e. constructors
    f. public methods
    g. Internal methods
    h. protected methods
    i. private methods
    ```

28. Function overloading must be avoided in most cases


    Use:
    ```cs
    public Anim GetAnimByIndex(int index);
    public Anim GetAnimByName(string name);
    ```

    Instead of:
    ```cs
    public Anim GetAnim(int index);
    public Anim GetAnim(string name);
    ```

29. Each class must be in a separate source file unless it makes sense to group several smaller classes.

30. The filename must be the same as the name of the class including upper and lower cases.

    ```cs
    public class PlayerAnimation {}
    ```
    
    PlayerAnimation.cs

31. When a class spans across multiple files(i.e. partial classes), these files have a name that starts with the name of the class, followed by a dot and the subsection name.

    ```cs
    public partial class Human;
    ```

    Human.Head.cs
    Human.Body.cs
    Human.Arm.cs

32. Use `assert` for any assertion you have. Assert is not recoverable. (e.g, most function will have `Debug.Assert`(not null parameters) )


33. The name of a bitflag enum must be suffixed by `Flags`

    ```cs
    [Flags]
    public enum EVisibilityFlags
    {
        None = 0,
        Character = 1 << 0,
        Terrain = 1 << 1,
        Building = 1 << 2,
    }
    ```

34. Prefer overloading over default parameters


35. When default parameters are used, restrict them to natural immutable constants such as `null`, `false` or `0`.


36. Shadowed variables are not allowed.

    ```cs
    public class SomeClass
    {
        public int Count { get; set; }
        public void Func(int count)
        {
            for (int count = 0; count != 10; ++count)
            {
                // Use count
            }
        }
    }
    ```

37. Always use containers from `System.Collections.Generic` over ones from `System.Collections`. Using a pure array is fine as well.

38. Prefer to use real type over implicit typing(i.e, `var`) unless the type is obvious from the right side of assignment, or when the type is unimportant. Some acceptable `var` usage includes `IEnumerable` and when the `new` keyword is on the same line, showing what type of object is being created clearly.

    ```cs
    var text = "string obviously";
    var age = 28;
    var employee = new Employee();

    string accountNumber = GetAccountNumber();
    ```


39. Use `static` class, not singleton pattern

40. Use `async Task` instead of `async void`. The only place where `async void` is allowed is for the event handler.


41. Validate any external data at the boundary and return before passing the data into our functions. This means that we assume all data is valid after this point.

42. Therefore, do not throw any exception from inside non-boundary methods. Also, exceptions should be handled at the boundary only.


43. As an exception to the previous rule, exception throwing is allowed when `switch`-`default` is used to catch missing `enum` handling logic. Still, do not catch this exception

    ```cs
    switch (accountType)
    {
        case AccountType.Personal:
            return something;
        case AccountType.Business:
            return somethingElse;
        default:
            throw new ArgumentOutOfRangeException(nameof(AccountType));
    }
    ```

44. Prefer not to allow `null` parameters in your function, especially from a `public` one.


45. If `null` parameter is used, and postfix the parameter name with `OrNull`

    ```cs
    public Anim GetAnim(string nameOrNull)
    {
    }
    ```

46. Prefer not to return `null` from any function, especially from a `public` one. However, you sometimes need to do this to avoid throwing exceptions.

47. If `null` is returned from any function. Postfix the function name with `OrNull`.

    ```cs
    public string GetNameOrNull();
    ```

48. Try not to use an object initializer. Use explicit constructor with named parameters instead. Two exceptions.
    a. When the object is created at one place only. (e.g, one-time DTO)
    b. When the object is created inside a static method of the owning class. (e.g, factory pattern)

## II. Code Formatting

1. Use Visual Studio's default for tabs. If another IDE is used, use 4 spaces instead of a real tab.

2. Always place an opening curly brace (`{`) in a new line


3. Add curly braces even if there's only one line in the scope

    ```cs
    if (bSomething)
    {
        return;
    }
    ```

4. Declare only one variable per line

    BAD:
    ```cs
    int counter = 0, index = 0;
    ```
    
    GOOD: 
    ```cs
    int counter = 0;
    int index = 0;
    ```

## III. Framework Specific Guidelines

### A. XAML Controls

1. Do not name (i.e, `x:name`) a control unless you absolutely need it


2. Use pascal casing with prefixed `x` character for the name.

    ```cs
    xLabelName
    ```

3. Prefix the name with full control type
    
    ```cs
    xLabelName
    xButtonAccept
    ```

### B. ASP .NET Core

1. When using DTO(Data Transfer Object)s for a request body for a RESTful API, make each value-type property as `nullable` so that model validation is automatic

    ```cs
    [Required]
    public Guid? ID { get; set; }
    ```

2. Validate all the requests as the first thing in any controller method. Once validation passes, all inputs are assumed to be correct. So no `[required]` `nullable` properties will be `null`.

3. Unlike above, `[RouteParam]` will not have `?`

    ```cs
    public bool GetAsync([RouteParam]Guid userID)
    ```


### C. Service/Repo Pattern

1. For the DTO classes and enums that are only used internally (e.g, internal microservice or DTO between service and repo), prefix it with `X`. This means they are transient classes and enums

    ```cs
    public sealed class XNode
    {
    }

    public enum EXTransactionStatus
    {
    }
    ```
