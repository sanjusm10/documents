## ***1. What is the use of private constructor***

##### 1. A class with a private constructor can not be inherited

##### 2. We can not create a object of the class which has private constructor

##### 3. Many times we do not want to create instances of certain classes like utility, common routine classes

In C#, a private constructor is used to restrict the instantiation of a class from outside the class itself. Here are some common uses of private constructors:

### **1. Singleton Pattern**

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

### **2. Static Classes**

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

### **3. Factory Methods**

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

### **4. Preventing Inheritance**

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

## ***2. IEnumerable and IEnumerator***

In C#, both `IEnumerable` and `IEnumerator` are interfaces used for iterating over collections. Here's a detailed look at each, including their uses and differences:

- IEnumerable does not remembers cursor state where as IEnumerator does.
- If your requirement is to just loop sequentially through collection one by one and you are not intersted in where the cursor is currently then IEnumerable is best fit. Syntax is much smaller.
- When you pass the IEnumerator from one function another function and remember the current cursor position then IEnumerator is best fit.
- IEnumerable uses IEnumerator internally.

### **IEnumerable**

- **Imp**: IEnumerable helps to iterate over a collection without knowing the actual type.It acts like an abstraction.
- **Namespace**: `System.Collections` or `System.Collections.Generic` (for generics).
- **Purpose**: Represents a collection that can be enumerated.
- **Key Method**: `GetEnumerator()`, which returns an `IEnumerator` object.
- **Usage**: Used with `foreach` loops for easy iteration over collections.

![IEnumerable](Images/IEnumerable.png)
**Example**:

```csharp
public class MyCollection : IEnumerable<int>
{
    private int[] _numbers = { 1, 2, 3, 4, 5 };

    public IEnumerator<int> GetEnumerator()
    {
        return ((IEnumerable<int>)_numbers).GetEnumerator();
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}

public class Program
{
    public static void Main()
    {
        MyCollection collection = new MyCollection();
        foreach (int number in collection)
        {
            Console.WriteLine(number);
        }
    }
}
```

### **IEnumerator**

- **Namespace**: `System.Collections` or `System.Collections.Generic` (for generics).
- **Purpose**: Provides the mechanism for iterating over a collection.
- **Key Methods**: `MoveNext()`, `Reset()`, and the `Current` property.
- **Usage**: Typically used internally by `IEnumerable` implementations.

![IEnumerator](Images/IEnumerator.png)

**Example**:

```csharp
public class MyCollectionEnumerator : IEnumerator<int>
{
    private int[] _numbers;
    private int _position = -1;

    public MyCollectionEnumerator(int[] numbers)
    {
        _numbers = numbers;
    }

    public bool MoveNext()
    {
        _position++;
        return (_position < _numbers.Length);
    }

    public void Reset()
    {
        _position = -1;
    }

    public int Current
    {
        get
        {
            if (_position < 0 || _position >= _numbers.Length)
                throw new InvalidOperationException();
            return _numbers[_position];
        }
    }

    object IEnumerator.Current => Current;

    public void Dispose()
    {
        // Dispose resources if needed
    }
}
```

### **Using IEnumerable and IEnumerator Together**

- **Implementation**: Typically, `IEnumerable` is implemented by a collection class to provide an enumerator (`IEnumerator`), allowing the collection to be iterated.
- **Example Implementation**:

```csharp
public class MyCollection : IEnumerable<int>
{
    private int[] _numbers = { 1, 2, 3, 4, 5 };

    public IEnumerator<int> GetEnumerator()
    {
        return new MyCollectionEnumerator(_numbers);
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}
```

### **Key Differences**

| Feature                  | IEnumerable                                   | IEnumerator                                     |
|--------------------------|-----------------------------------------------|-------------------------------------------------|
| **Purpose**              | Represents a collection that can be iterated | Provides the mechanism for iterating a collection |
| **Key Method/Property**  | `GetEnumerator()`                             | `MoveNext()`, `Reset()`, `Current`              |
| **Usage**                | Used with `foreach` loops                     | Used internally by `IEnumerable` implementations |
| **Namespace**            | `System.Collections`, `System.Collections.Generic` | `System.Collections`, `System.Collections.Generic` |

### **When to Use Each**

- **IEnumerable**: Use when you need to represent a collection that can be iterated over. It is commonly used for collections and enables `foreach` loop syntax.
- **IEnumerator**: Use when you need to implement the actual iteration mechanism for a collection. It is typically used internally within classes that implement `IEnumerable`.

Understanding `IEnumerable` and `IEnumerator` helps you create custom collections and iterators, allowing you to control how collections are traversed and interacted with.

## ***3. IEnumerable versus IQueryable***

In C#, both `IEnumerable` and `IQueryable` are used to represent collections of objects, but they have different use cases, performance characteristics, and behaviors. Here’s a detailed comparison to help you understand their differences and when to use each one:

### **IEnumerable**

- **Namespace**: `System.Collections`
- **Use Case**: Suitable for in-memory collections like arrays, lists, and other collections that are enumerated sequentially.
- **Execution**: Executes queries against the in-memory data. All operations are performed in-memory, which means the entire collection is loaded into memory.
- **Deferred Execution**: Supports deferred execution, meaning the query is not executed until the data is enumerated (e.g., using `foreach` loop).
- **Extension Methods**: LINQ extension methods for `IEnumerable` are defined in `System.Linq.Enumerable`.

![IEnumerableClientFilter](Images/IEnumerableClientFilter.png)

**Example**:

```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
IEnumerable<int> query = numbers.Where(n => n > 3);

foreach (int number in query)
{
    Console.WriteLine(number); // Output: 4, 5
}
```

### **IQueryable**

- **Namespace**: `System.Linq`
- **Use Case**: Suitable for querying data from remote data sources like databases. It enables efficient querying by translating queries to the underlying data source.
- **Execution**: Executes queries against a remote data source. The query is translated to the appropriate query language (e.g., SQL for databases) and executed on the data source.
- **Deferred Execution**: Supports deferred execution, similar to `IEnumerable`.
- **Extension Methods**: LINQ extension methods for `IQueryable` are defined in `System.Linq.Queryable`.

![IQueryableDatabaseFilter](Images/IQueryableDatabaseFilter.png)

**Example**:

```csharp
using (var context = new MyDbContext())
{
    IQueryable<int> query = context.Numbers.Where(n => n > 3);

    foreach (int number in query)
    {
        Console.WriteLine(number); // Output: 4, 5 (fetched from the database)
    }
}
```

### **Key Differences**

| Feature                | IEnumerable                                         | IQueryable                                         |
|------------------------|-----------------------------------------------------|---------------------------------------------------|
| **Namespace**          | `System.Collections`                                | `System.Linq`                                     |
| **Use Case**           | In-memory collections                               | Remote data sources (e.g., databases)             |
| **Execution**          | In-memory                                           | Remote data source                                |
| **Query Translation**  | Not translated                                      | Translated to the underlying data source query    |
| **Deferred Execution** | Yes                                                 | Yes                                               |
| **Extension Methods**  | `System.Linq.Enumerable`                            | `System.Linq.Queryable`                           |

### **When to Use Each**

- **Use `IEnumerable`** when working with in-memory collections where the entire collection is already loaded into memory and you want to perform operations on it.
- **Use `IQueryable`** when working with remote data sources like databases where you want to leverage the querying capabilities of the data source to fetch and filter data efficiently.

By understanding these differences, you can make informed decisions on which interface to use based on your specific needs and the characteristics of your data source.

## ***4. Yield keyword***

The `yield` keyword in C# is used to simplify the creation of iterators, which are used to traverse collections or streams of data. It enables the implementation of a stateful iterator method without the need to explicitly maintain the state and manage the enumeration logic.

![yield](Images/Yield.png)

Here are some real-time usage examples:

### **1. Simplifying Iterators**

The `yield` keyword is used to produce elements one at a time, as they are needed. This approach can simplify the code and improve readability.

**Example**:

```csharp
public IEnumerable<int> GetEvenNumbers(int max)
{
    for (int i = 0; i <= max; i += 2)
    {
        yield return i;
    }
}

public static void Main()
{
    var evenNumbers = GetEvenNumbers(10);
    foreach (var num in evenNumbers)
    {
        Console.WriteLine(num); // Output: 0, 2, 4, 6, 8, 10
    }
}
```

### **2. Infinite Sequences**

The `yield` keyword can be used to generate infinite sequences in a memory-efficient way.

**Example**:

```csharp
public IEnumerable<int> GetFibonacciSequence()
{
    int a = 0;
    int b = 1;
    while (true)
    {
        yield return a;
        int temp = a;
        a = b;
        b = temp + b;
    }
}

public static void Main()
{
    var fibonacci = GetFibonacciSequence().Take(10);
    foreach (var num in fibonacci)
    {
        Console.WriteLine(num); // Output: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
    }
}
```

### **3. Filtering Data Streams**

The `yield` keyword can be used to filter data streams efficiently.

**Example**:

```csharp
public IEnumerable<int> FilterData(IEnumerable<int> data, Func<int, bool> predicate)
{
    foreach (var item in data)
    {
        if (predicate(item))
        {
            yield return item;
        }
    }
}

public static void Main()
{
    var numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    var filteredNumbers = FilterData(numbers, n => n > 5);

    foreach (var num in filteredNumbers)
    {
        Console.WriteLine(num); // Output: 6, 7, 8, 9, 10
    }
}
```

### **4. State Machines**

The `yield` keyword can be used to implement state machines, making it easier to manage state transitions.

**Example**:

```csharp
public IEnumerable<string> StateMachine()
{
    yield return "State 1";
    yield return "State 2";
    yield return "State 3";
}

public static void Main()
{
    var states = StateMachine();
    foreach (var state in states)
    {
        Console.WriteLine(state); // Output: State 1, State 2, State 3
    }
}
```

### **How It Works**

- **`yield return`**: Returns each element one at a time and maintains the current state of the method.
- **State Management**: The compiler generates a state machine behind the scenes, allowing the method to pause and resume execution, making it easier to handle complex iterations.

### **Benefits**

- **Simplicity**: Simplifies the code by removing the need to manually manage state and iterators.
- **Efficiency**: Produces elements as needed, which can improve memory usage and performance.
- **Readability**: Makes the code more readable and maintainable by abstracting the complexity of iterators.

Using the `yield` keyword can greatly simplify the process of creating iterators and handling sequences, making your code more efficient and easier to understand.

## ***5.Constant and ReadOnly in C#***

In C#, both `const` and `readonly` are used to define values that should not change after they are set, but they have some key differences in their usage and behavior. Here's a detailed comparison:

### **1. `const` (Constant)**

- **Definition**: Constants are fixed values that cannot be changed after they are declared. They are implicitly static and must be initialized at the time of declaration.
- **Scope**: Can only be used to define constants of primitive types (such as `int`, `float`, `char`, etc.), enums, or string literals.
- **Initialization**: Must be initialized at the time of declaration and cannot be modified thereafter.

**Example**:

```csharp
public class MyClass
{
    public const int MyConstant = 10;
}
```

In this example, `MyConstant` is a constant value that is set to `10` and cannot be changed.

### **2. `readonly`**

- **Definition**: Readonly fields can be assigned a value either at the time of declaration or in a constructor. Once assigned, their values cannot be changed.
- **Scope**: Can be used to define fields of any type, including complex types like objects and arrays.
- **Initialization**: Can be initialized at the time of declaration or within a constructor, providing more flexibility compared to `const`.

**Example**:

```csharp
public class MyClass
{
    public readonly int MyReadonlyField;

    // Initialize in the constructor
    public MyClass(int value)
    {
        MyReadonlyField = value;
    }
}
```

In this example, `MyReadonlyField` is a readonly field that is assigned a value in the constructor and cannot be modified thereafter.

### **Key Differences**

| Feature             | `const`                                    | `readonly`                                     |
|---------------------|--------------------------------------------|------------------------------------------------|
| **Initialization**  | At declaration                             | At declaration or in a constructor             |
| **Value Type**      | Primitive types, enums, string literals    | Any type (primitive, complex objects, arrays)  |
| **Mutability**      | Immutable after declaration                | Immutable after assignment                     |
| **Scope**           | Implicitly static, accessed via class name | Instance-specific or static                    |
| **Flexibility**     | Less flexible (must be assigned at declaration) | More flexible (can be assigned in constructors) |

### **Usage Scenarios**

- **Use `const`**: When you have values that are known at compile-time and will never change, such as mathematical constants (e.g., `Pi`) or fixed configuration values.
- **Use `readonly`**: When you need values that can be assigned at runtime, such as configuration settings read from a file or values passed through constructors that should remain constant after initialization.

### **Example Usage**

```csharp
public class Config
{
    // Constant value
    public const string AppName = "MyApplication";

    // Readonly value assigned through constructor
    public readonly string ConnectionString;

    public Config(string connectionString)
    {
        ConnectionString = connectionString;
    }
}

public class Program
{
    public static void Main()
    {
        Config config = new Config("Server=myServer;Database=myDB;User=myUser;Password=myPass;");
        Console.WriteLine(Config.AppName); // Output: MyApplication
        Console.WriteLine(config.ConnectionString); // Output: Server=myServer;Database=myDB;User=myUser;Password=myPass;
    }
}
```

By understanding the differences and appropriate usage of `const` and `readonly`, you can write more robust and maintainable code.

## ***6.Difference between Abstraction and Encapsulation***

Abstraction and encapsulation are both fundamental concepts in object-oriented programming (OOP), but they serve different purposes. Let's explore the key differences between them:

### **Abstraction**

- **Purpose**: Abstraction focuses on hiding the complex implementation details and showing only the essential features of an object. It simplifies the complexity by exposing only what is necessary.
- **Implementation**: Achieved through abstract classes and interfaces.
- **Usage**: Helps in defining a clear contract or blueprint for a class without specifying the internal workings. It allows different implementations to follow the same contract.
- **Example**: Abstracting the concept of a `Vehicle` class, showing only essential attributes like `speed` and `fuelCapacity`, without detailing the inner workings of each type of vehicle.

**Example**:

```csharp
public abstract class Vehicle
{
    public int Speed { get; set; }
    public int FuelCapacity { get; set; }

    public abstract void StartEngine();
    public abstract void StopEngine();
}
```

In this example, the `Vehicle` class abstracts the concept of a vehicle, specifying essential features without detailing the implementation.

### **Encapsulation**

- **Purpose**: Encapsulation is about bundling the data (attributes) and methods (functions) that operate on the data into a single unit, or class. It also restricts direct access to some of an object's components, which can prevent accidental or unauthorized modifications.
- **Implementation**: Achieved using access modifiers like `private`, `protected`, and `public`.
- **Usage**: Ensures that the internal representation of an object is hidden from the outside, allowing controlled access through public methods (getters and setters).
- **Example**: Encapsulating the properties of a `BankAccount` class to restrict direct access and modification of the account balance.

**Example**:

```csharp
public class BankAccount
{
    private decimal balance;

    public decimal Balance
    {
        get { return balance; }
        private set { balance = value; }
    }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
            Balance += amount;
        }
    }

    public void Withdraw(decimal amount)
    {
        if (amount > 0 && amount <= Balance)
        {
            Balance -= amount;
        }
    }
}
```

In this example, the `BankAccount` class encapsulates the `balance` property, providing controlled access through the `Deposit` and `Withdraw` methods.

### **Key Differences**

| Feature             | Abstraction                                        | Encapsulation                                     |
|---------------------|----------------------------------------------------|---------------------------------------------------|
| **Purpose**         | Hide complex implementation, show essential features | Bundle data and methods, restrict access          |
| **Implementation**  | Abstract classes and interfaces                    | Access modifiers (private, protected, public)     |
| **Focus**           | Defining a clear contract or blueprint              | Protecting and controlling access to data         |
| **Example**         | Abstract class `Vehicle`                           | Class `BankAccount` with private balance          |

### **Summary**

- **Abstraction**: Simplifies complexity by exposing only essential details and hiding the implementation. It is implemented using abstract classes and interfaces.
- **Encapsulation**: Bundles data and methods into a single unit and restricts direct access to some components. It is implemented using access modifiers to control visibility.

Understanding these concepts helps in designing robust and maintainable object-oriented systems, ensuring clear separation of concerns and protecting the integrity of data.
