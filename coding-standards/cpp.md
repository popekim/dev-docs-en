---
title: "C++ Coding Standards"
date: 2021-05-25
---

## Preface

### Rule of Thumb

1. Readability first (your code should be your documentation most of the time)
2. Crash/Assert early. Don't wait until the worst case happens to make the crash condition.
3. Follow IDE's auto formatted style unless you have really good reasons not to do so. (Ctrl + K + D in VC++)
4. Learn from existing code

### References
This coding standards is inspired by these coding standards
* [Unreal engine 4 coding standard](https://docs.unrealengine.com/latest/INT/Programming/Development/CodingStandard/)
* [Doom 3 Code Style Conventions](ftp://ftp.idsoftware.com/idstuff/doom3/source/codestyleconventions.doc)
* [IDesign C# Coding Standard](http://www.idesign.net/downloads/getdownload/1985)

## I. Main Coding Standards
1. Use Pascal casing for class and structs
    
    ```cpp
    class PlayerManager;
    struct AnimationInfo;
    ```

2. Use camel casing for local variable names and function parameters
    
    ```cpp
    void SomeMethod(const int someParameter);
    {
        int someNumber;
        int id;
    }
    ```

3. Use verb-object pairs for method names
    a. Use pascal casing for public methods

        ```cpp
        public:
            void DoSomething();
        ```

    b. Use camel casing for other methods
    
        ```cpp
        private:
            void doSomething();
        ```

4. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for constants and defines

    ```cpp
    constexpr int SOME_CONSTANT = 1;
    ```


5. Use all lowercase letters for namespaces

    ```cpp
    namespace abc{};
    ```

6. prefix boolean variables with `b`

    ```cpp
    bool bFired;	// for local and public member variable
    bool mbFired;	// for private class member variable
    ```


7. prefix interfaces with `I`

    ```cpp
    class ISomeInterface;
    ```

8. prefix enums with `e`

    ```cpp
    enum class eDirection
    {
        North,
        South
    }
    ```

9. prefix class member variables with `m`.

    ```cpp
    class Employee
    {
    protected:
        int mDepartmentID;
    private:
        int mAge;
    }
    ```

10. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for `goto` labels.

    ```cpp
    goto MY_LABEL;


    // ....

    MY_LABEL:
        std::cout << "Magic!" << std::endl;
        return 0;
    ```

11. Methods with return values must have a name describing the value returned

    ```cpp
    uint32_t GetAge() const;
    ```

12. Use descriptive variable names. e.g `index` or `employee` instead of `i` or `e` unless it is a trivial index variable used for loops.

13. Capitalize every characters in acronyms only if there is no extra word after them.

    ```cpp
    int OrderID;
    int HttpCode;
    ```

14. Always use setter and getters for class member variables

    Use:
    ```cpp
    class Employee
    {
    public:
        const string& GetName() const;
        void SetName(const string& name);
    private:
        string mName;
    }
    ```

    Instead of:
    ```cpp
    class Employee
    {
    public:
        string Name;
    }
    ```


15. Use only public member variables for a struct. No functions are allowed. Use pascal casing for the members of a struct.

    ```cpp
    struct MeshData
    {
        int32_t VertexCount;
    }
    ```

16. Use `#include<>` for external header files. Use `#include ""` for in-house header files

17. Put external header files first, followed by in-house header files in alphabetic order if possible.

    ```cpp
    #include <vector>
    #include <unordered_map>

    #include "AnimationInfo.h"
    ```

18. Use `#pragma once` at the beginning of every header file

19. Declare local variables as close as possible to the first line where it is being used.

20. Use precision specification for floating point values unless there is an explicit need for a `double`

    ```cpp
    float f = 0.5f;
    ```

21. Always have a `default` case for a `switch` statement.

    ```cpp
    switch (number)
    {
    case 0:
        ... 
        break;
    default:
        break;
    }
    ```

22. Always add predefined FALLTHROUGH for switch case fall through unless there is no code in the `case` statement. This will be replaced by `[[fallthrough]]` attribute coming in for C++17 later

    ```cpp
    switch (number)
    {
    case 0:
        DoSomething();
        FALLTHROUGH
    case 1:
        DoFallthrough();
        break;
    case 2:
    case 3:
        DoNotFallthrough();
        break;
    default:
        break;
    }
    ```

23. If default case must not happen in a `switch` case, always add `Assert(false)`. In our assert implementation, this will add optimization hint for release build.

    ```cpp
    switch (type)
    {
    case 1:
        ... 
        break;
    default:
        Assert(false, "unknown type");
        break;
    }
    ```

24. Use `const`s as much as possible even for local variable and function parameters.


25. Any member functions that doesn't modify the object must be `const`

    ```cpp
    int GetAge() const;
    ```

26. Do not return const value type. `Const` return is only for reference and pointers

27. Names of recursive functions end with `Recursive`.

    ```cpp
    void FibonacciRecursive();
    ```

28. Order of class variables and methods must be as follows:
    
    a. list of friend classes
    b. public methods
    c. protected methods
    d. private methods
    e. protected variables
    f. private variables


29. Function overloading must be avoided in most cases

    Use:
    ```cpp
    const Anim* GetAnimByIndex(const int index) const;
    const Anim* GetAnimByName(const char* name) const;
    ```

    Instead of:
    ```cp
    const Anim* GetAnim(const int index) const;
    const Anim* GetAnim(const char* name) const;
    ```

30. Overloading functions to add `const` accessible function is allowed.

    ```cpp 
    Anim* GetAnimByIndex(const int index);
    const Anim* GetAnimByIndex(const int index) const;
    ```

31. Avoid use of `const_cast`. Instead create a function that clearly returns an editable version of the object

32. Each class must be in a separate source file unless it makes sense to group several smaller classes.


33. The filename must be the same as the name of the class including upper and lower cases.

    ```cpp
    class PlayerAnimation;

    PlayerAnimation.cpp
    PlayerAnimation.h
    ```

34. When a class spans across multiple files, these files have a name that starts with the name of the class, followed by an underscore and the subsection name.

    ```cpp
    class RenderWorld;

    RenderWorld_load.cpp
    RenderWorld_demo.cpp
    RenderWorld_portals.cpp
    ```

35. Platform specific class for [reverse OOP](https://www.youtube.com/watch?v=GnWASmocihE) pattern uses similar naming convention

    ```cpp
    class Renderer;

    Renderer.h		// all renderer interfaces called by games
    Renderer.cpp	// Renderer's Implementations which are
                // to all platforms
    Renderer_gl.h	// RendererGL interfaces called by
                // Renderer
    Renderer_gl.cpp	// RendererGL implementations
    ```

36. Use our own version of Assert instead of standard c assert

37. Use assert for any assertion you have. Assert is not recoverable. This can be replaced by compiler optimization hint keyword [__assume](https://docs.microsoft.com/en-us/cpp/intrinsics/assume) for the release build.


38. Any memory allocation must be done through our own New and Delete keyword.


39. Memory operations such as memset, memcpy and memmove also must be done through our own MemSet, MemCpy and MemMove.


40. Generally prefer reference(`&`) over pointers unless you need `nullptr` for any reason. (exceptions are mentioned right below)

41. Use pointers for `out` parameters. Also prefix the function parameters with out.

    function:
    ```cpp
    void GetScreenDimension(uint32_t* const outWidth, uint32_t* const  outHeight)
    {
    }
    ```

    caller:
    ```cpp
    uint32_t width;
    uint32_t height;
    GetScreenDimension(&width, &height);
    ```

42. The above out parameters must not be null. (Use assert, not if statement)

    ```cpp
    void GetScreenDimension(uint32_t* const outWidth, uint32_t* const  outHeight)
    {
        Assert(outWidth);
        Assert(outHeight);
    }
    ```

43. Use pointers if the parameter will be saved internally.

    ```cpp
    void AddMesh(Mesh* const mesh)
    {
        mMeshCollection.push_back(mesh);
    }
    ```

44. Use pointers if the parameter should be generic `void*` parameter

    ```cpp
    void Update(void* const something)
    {
    }
    ```

45. The name of a bitflag enum must be suffixed by `Flags`
    
    ```cpp
    enum class eVisibilityFlags
    {
    }
    ```


46. Do not add size specifier for `enum` unless you need that specific size (e.g, for serialization of data members)

    ```cpp
    enum class eDirection : uint8_t
    {
        North,
        South
    }
    ```

47. Prefer overloading over default parameters


48. When default parameters are used, restrict them to natural immutable constants such as `nullptr`, `false` or `0`.


49. Prefer fixed-size containers whenever possible.


50. `reserve()` dynamic containers whenever possible


51. Always put parentheses for defined numbers 

    ```cpp
    #define NUM_CLASSES (1)
    ```

52. Prefer constants over defines


53. Always use forward declaration if possible instead of using includes


54. All compiler warnings must be addressed.


55. Shadowed variables are not allowed.

    ```cpp
    class SomeClass
    {
    public:
        int32_t Count;
    public:
        void Func(const int32_t Count)
        {
            for (int32_t count = 0; count != 10; ++count)
            {
                // Use Count
            }
        }
    }
    ```


56. Declare only one variable per line

    BAD:
    ```cpp
    int counter, index;
    ```

    GOOD: 
    ```cpp
    int counter;
    int index;
    ```

57. Take advantage of [NRVO](https://docs.microsoft.com/en-us/previous-versions/ms364057(v=vs.80)), when you are returning a local object. This means you need to have only one return statement inside your function. This applies only when you return an object by value.


58. Do not use `const` member variables in a `struct` or `class` simply to prevent modification after initialization. Same rule is applied to reference(`&`) member variables.


59. When initializing member variables, use initializer list over assignments, by default.


60. <<<__restrict keyword

## II. Code Formatting

1. There must be a blank line between includes and body.


2. Use Visual Studio default for tabs. If you are not using Visual Studio, use real tabs that are equal to 4 spaces.


3. Always place an opening curly brace (`{`) in a new line


4. Add curly braces even if there's only one line in the scope

    ```cpp
    if (bSomething)
    {
        return;
    }
    ```

5. Put pointer or reference sign right next to the type

    ```cpp
    int& number;
    int* number;
    ```

6. When initializing member variables via initializer list, initialize one member variable per line and follow below format.

    BAD:
    ```cpp
    MyClass::MyClass(const int var1, const int var2)
        :mVar1(var1), mVar2(var2), mVar3(0)
    {
    ```

    GOOD:
    ```cpp
    MyClass::MyClass(const int var1, const int var2)
        : mVar1(var1)
        , mVar2(var2)
        , mVar3(0)
        {
    ```

## III. Modern Language Features

1. `override` and `final` keywords are mandatory


2. Use `enum class` always

    ```cpp
    enum class eDirection
    {
        North,
        South
    }
    ```

3. Use `static_assert` over `Assert`, if possible.


4. Use `nullptr` over `NULL`


5. Use `unique_ptr` when a object lifetime is solely handled inside a class. (i.e. new in constructor delete in destructor)


6. Range-based `for` are recommended where applicable


7. Do not use `auto` unless it is for a iterator or `new` keyword is on the same line, showing which object is created clearly


8. Do not manually perform return value optimization using `std::move`. It breaks automatic [NRVO optimization](https://msdn.microsoft.com/en-us/library/ms364057(v=vs.80).aspx).


9. Move constructor and move assignment operator are allowed.


10. Use `constexpr` instead of const for simple constant variables

    ```cpp
    constexpr int DEFAULT_BUFFER_SIZE = 65536
    ```

    Instead of

    ```cpp
    const int DEFAULT_BUFFER_SIZE = 65536
    ```

11. <<<TBD: constexpr

12. <<<TBD: Lambda

13. <<<TBD: do not use shared_ptr


## IV. Project Settings and Project Structure
1. Visual C++: Always use property sheets to change project settings

2. Do not disable compile warnings in project settings. Use #pragma in code instead.
