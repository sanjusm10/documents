<details open>
  <summary> 1. What is the use of private constructor </summary>
  
 1. A class with a private constructor can not be inherited

 2. We can not create a object of the class which has private constructor

 3. Many times we do not want to create instances of certain classes like utility, common routine classes

In C#, a private constructor is used to restrict the instantiation of a class from outside the class itself. Here are some common uses of private constructors:

###### **1. Singleton Pattern**

A private constructor is often used in the Singleton design pattern to ensure that only one instance of the class can be created. This is useful when you need a single, shared instance of a class throughout the application.

**Example**:

```csharp
public class Singleton
{
    private static Singleton _instance;
    
    // Private constructor
    private Singleton() { }
    
    public static Singleton Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new Singleton();
            }
            return _instance;
        }
    }
}
```

###### **2. Static Classes**

A private constructor can be used to prevent the instantiation of a static class. Static classes cannot be instantiated, and by declaring a private constructor, you ensure that no instances of the class are created accidentally.

**Example**:

```csharp
public static class Utility
{
    // Private constructor
    private Utility() { }

    public static void DoSomething()
    {
        // Implementation
    }
}
```

###### **3. Factory Methods**

Private constructors can be used in combination with factory methods to control the creation of instances. Factory methods allow you to create instances of a class in a controlled manner, often with additional logic.

**Example**:

```csharp
public class Product
{
    public string Name { get; private set; }

    // Private constructor
    private Product(string name)
    {
        Name = name;
    }

    // Factory method
    public static Product CreateProduct(string name)
    {
        // Additional logic before creating an instance
        return new Product(name);
    }
}
```

###### **4. Preventing Inheritance**

By declaring a private constructor, you can prevent a class from being inherited. This can be useful when you want to ensure that a class remains sealed and cannot be extended.

**Example**:

```csharp
public class NonInheritable
{
    // Private constructor
    private NonInheritable() { }

    public static void DoSomething()
    {
        // Implementation
    }
}
```

In summary, private constructors are a powerful tool in C# that provide fine-grained control over how and when instances of a class can be created. They are commonly used in design patterns, static classes, factory methods, and to prevent inheritance.

</details>
