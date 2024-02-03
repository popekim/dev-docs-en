---
title: "C# Coding Standards"
date: 2024-02-02
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

1. Use camel casing for local variable names and function parameters

   ```cs
   public void SomeMethod(int someParameter)
   {
       int someNumber;
       int id;
   }
   ```


1. Use verb(base form)-object pairs for method names, by default.

    ```cs
    public uint GetAge()
    {
        // function implementation...
    }
    ```


1. However, if a method simply returns a boolean state, the verb part of the name should be prefixed Is, Can, Has or Should. If the function name becomes not natural by doing so, use the 3rd-person singular form of another verb.

    ```cs
    public bool IsAlive(Person person);
    public bool Has(Person person);
    public bool CanAccept(Person person);
    public bool ShouldDelete(Person person);
    public bool Exists(Person person);
    ```

1. Use pascal casing for all method names except (see below)

    ```cs
    public uint GetAge()
    {
        // function implementation...
    }
    ```

1. Use camel case for any non-public method. You might need to add custom Visual Studio style rule as described here

    ```cs
    private uint getAge()
    {
        // function implementation...
    }
    ```

1. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for constants

    ```cs
    const int SOME_CONSTANT = 1;
    ```

1. Use static readonly if an object is a constant

   ```cs
   public static readonly MyConstClass MY_CONST_OBJECT = new MyConstClass();
   ```

1. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for `static readonly` variables used as constants

1. Use `readonly` when a variable must be assigned only once

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

1. Use pascal casing for namespaces
    
    ```cs
    namespace System.Graphics
    ```

1. prefix boolean variables with `b`.

    ```cs
    bool bFired;		// for local variable
    private bool mbFired;	// for private member variable
    ```

1. prefix boolean properties with `Is`, `Can`, `Should` or `Has`. 

    ```cs
    public bool IsFired { get; private set; }
    public bool HasChild { get; private set; }
    public bool CanModal { get; private set; }
    public bool ShouldRedirect { get; private set; }
    ```

1. prefix interfaces with `I`

    ```cs
    interface ISomeInterface;
    ```

1. prefix enums with `E`

    ```cs
    public enum EDirection
    {
        North,
        South
    }
    ```

1. prefix structs with `S` unless they are `readonly struct`s

    ```cs
    public struct SUserID;
    ```

1. prefix `private` member variables with `m`. Use Pascal casing for the rest of a member variable
    
    ```cs
    Public class Employee
    {
        public int DepartmentID { get; set; }
        private int mAge;
    }
    ```

1. Methods with return values must have a name describing the value returned

    ```cs
    public uint GetAge();
    ```

1. Use descriptive variable names. e.g `index` or `employee` instead of `i` or `e` unless it is a trivial index variable used for loops.

1. Capitalize every character in acronyms only if there is no extra word after them.

    ```cs
    public int OrderID { get; private set; }
    public string HttpAddress { get; private set; }
    ```

1. Prefer properties over getter setter functions


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

1. Declare local variables as close as possible to the first line where it is being used.


1. Use precision specification for floating point values unless there is an explicit need for a `double`.

    ```cs
    float f = 0.5F;
    ```

1. Always have a `default`: case for a `switch` statement.

    ```cs
    switch (number)
    {
        case 0:
            ... 
            break;
        default:
            break;
    ```


1. If `default`: case must not happen in a `switch` case, always add `Debug.Fail();`

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

1. `Debug.Assert()` every assumptions you make while writing code.

1. Names of recursive functions end with `Recursive`

    ```cs
    public void FibonacciRecursive();
    ```

1. Order of class variables and methods must be as follows:

    1. member variables
    2. properties (exception: if a private variable is accessed by a property, it should appear right before the mapped property)
    3. constructors
    4. methods (follow public to private order)

1. In a class, group relevant methods together. Aslo group relevant member variables together.

1. If parameter types are general, function overloading must be avoided


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

1. Each class must be in a separate source file unless it makes sense to group several smaller classes.

1. The filename must be the same as the name of the class including upper and lower cases.

    ```cs
    public class PlayerAnimation {}
    ```
    
    PlayerAnimation.cs

1. When a class spans across multiple files(i.e. partial classes), these files have a name that starts with the name of the class, followed by a dot and the subsection name.

    ```cs
    public partial class Human;
    ```

    Human.Head.cs
    Human.Body.cs
    Human.Arm.cs

1. Use `assert` for any assertion you have. Assert is not recoverable. (e.g, most function will have `Debug.Assert`(not null parameters) )

1. The name of a bitflag enum must be suffixed by `Flags`

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

1. Prefer overloading over default parameters

1. When default parameters are used, restrict them to natural immutable constants such as `null`, `false` or `0`.

1. Shadowed variables are not allowed.

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

1. Always use containers from `System.Collections.Generic` over ones from `System.Collections`. Using a pure array is fine as well.

1. Use real type over implicit typing(i.e, `var`) unless the type is unimportant. Some acceptable `var` usage includes `IEnumerable` and when the `new` keyword is used for anonymous type.

1. Use `static` class, not singleton pattern

1. Use `async Task` instead of `async void`. The only place where `async void` is allowed is for the event handler.

1. Do not add `-Async` postfix for `async` methods.

1. Validate any external data at the boundary and return before passing the data into our functions. This means that we assume all data is valid after this point.

1. Therefore, do not throw any exception from inside non-boundary methods. Also, exceptions should be handled at the boundary only.

1. As an exception to the previous rule, exception throwing is allowed when `switch`-`default` is used to catch missing `enum` handling logic. Still, do not catch this exception

    ```cs
    switch (accountType)
    {
        case AccountType.Personal:
            return something;
        case AccountType.Business:
            return somethingElse;
        default:
            throw new NotImplementedException($"unhandled switch case: {accountType}");
    }
    ```

1. Prefer not to allow `null` parameters in your function, especially from a `public` one.

1. If `null` parameter is used, and postfix the parameter name with `OrNull`

    ```cs
    public Anim GetAnim(string nameOrNull)
    {
    }
    ```

1. Prefer not to return `null` from any function, especially from a `public` one. However, you sometimes need to do this to avoid throwing exceptions.

1. If `null` is returned from any function. Postfix the function name with `OrNull`.

    ```cs
    public string GetNameOrNull();
    ```

1. Utilize in-line Lambda expressions exclusively for single, straightforward statements.

1. Avoid object initializer unless it is used with `required` modifier(C# 11.0) and init-only setter (C# 9.0)

1. Declare the variable for an `out` parameter on a seprate line. Do NOT declare it int the argument list.

1. Do not use the null coalescing operator, introduced in C# 7.0.

1. Do not use `using` declaration, introduced in C# 8.0. Use `using` statement instead.

1. Always specify a data type after `new` keyword unless you are using annoymous type inside a function.

1. Use private init-only setter(`private init`), introduced in C# 9.0, wherever possible.

1. Use file scoped namespace declarations, introduced in C# 10.0.

1. When strong-typing a generic type, use `readonly record struct`, introduced in C# 10.0.

## II. Code Formatting

1. Use Visual Studio's default for tabs. If another IDE is used, use 4 spaces instead of a real tab.

1. Always place an opening curly brace (`{`) in a new line


1. Add curly braces even if there's only one line in the scope

    ```cs
    if (bSomething)
    {
        return;
    }
    ```

1. Declare only one variable per line

    BAD:
    ```cs
    int counter = 0, index = 0;
    ```
    
    GOOD: 
    ```cs
    int counter = 0;
    int index = 0;
    ```

## III. Project Settings
1. For Release builds, treat compiler warnings as errors.

1. Do not use implicit global using (C# 10.0)

## IV. Framework Specific Guidelines

### A. Auto Serialization/Deserialization (e.g. `System.Text.Json`)

1. Auto-serializable data must be defined as `class`.

1. Auto-serializable `class` must not contain any library-specific attribute in it.

1. All data in auto-serializable `class` must be declared/defined via `public` auto properties. (1-to-1 mapping between properties and member variables)

1. If you need a read-only property in auto-serializable `class`, make a `public` method instead.

1. Auto-serializable `class` must have only one `public` constructor. This constructor must not take any parameter.

1. Do not directly call a auto-serialization method. (e.g. `JsonSerializer.Serialize<>()`). Make a wrapper method instead to limit the parameter types.

### B. XAML Controls

1. Do not name (i.e, `x:name`) a control unless you absolutely need it


1. Use pascal casing with prefixed `x` character for the name.

    ```cs
    xLabelName
    ```

1. Prefix the name with full control type
    
    ```cs
    xLabelName
    xButtonAccept
    ```

### C. ASP .NET Core

1. When using DTO(Data Transfer Object)s for a request body for a RESTful API, make each value-type property as `nullable` so that model validation is automatic

    ```cs
    [Required]
    public Guid? ID { get; set; }
    ```

1. Validate all the requests as the first thing in any controller method. Once validation passes, all inputs are assumed to be correct. So no `[required]` `nullable` properties will be `null`.

1. Unlike above, `[RouteParam]` will not have `?`

    ```cs
    public bool GetUser([RouteParam]Guid userID)
