## ***1.SOLID Design Principles***

The SOLID principles are a set of five design principles intended to make software designs more understandable, flexible, and maintainable. These principles were introduced by Robert C. Martin and are widely used in object-oriented programming, including C#. Let's dive into each principle in detail:

### 1. **Single Responsibility Principle (SRP)**
**Definition**: A class should have only one reason to change, meaning it should have only one responsibility or job.

**Explanation**: This principle ensures that a class is focused on a single task or functionality. By doing so, the class becomes easier to understand, test, and maintain. If a class has multiple responsibilities, changes in one area might affect the other areas, leading to a fragile design.

**Example**:
```csharp
public class ReportGenerator
{
    public void GenerateReport()
    {
        // Code to generate report
    }
}

public class ReportPrinter
{
    public void PrintReport(ReportGenerator report)
    {
        // Code to print report
    }
}
```
In this example, `ReportGenerator` is responsible only for generating reports, and `ReportPrinter` is responsible only for printing reports.

![SRP](Images/SRP.png)

### 2. **Open/Closed Principle (OCP)**
**Definition**: Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.

**Explanation**: This principle states that you should be able to extend the behavior of a class without modifying its existing code. This can be achieved through inheritance and polymorphism, allowing new functionality to be added by creating new subclasses.

**Example**:
```csharp
public abstract class Employee
{
    public abstract double CalculateBonus(double salary);
}

public class PermanentEmployee : Employee
{
    public override double CalculateBonus(double salary)
    {
        return salary * 0.1;
    }
}

public class ContractEmployee : Employee
{
    public override double CalculateBonus(double salary)
    {
        return salary * 0.05;
    }
}
```
In this example, the `Employee` class is open for extension by creating new subclasses (`PermanentEmployee`, `ContractEmployee`) without modifying the existing `Employee` class.

- The benefit is simple testing is required to test individual classes but if you will keep on adding and modifying in one class. Then even for the smallest modification, the whole class needs to be tested

![OCP1](Images/OCP1.png)
![OCP2](Images/OCP2.png)

### 3. **Liskov Substitution Principle (LSP)**
**Definition**: Subtypes must be substitutable for their base types without altering the correctness of the program.

**Explanation**: This principle ensures that a derived class can be used in place of its base class without causing errors or unexpected behavior. The derived class should enhance or extend the functionality of the base class without changing its essential behavior.

**Example**:
```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int GetArea()
    {
        return Width * Height;
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = base.Height = value; }
    }
}
```
In this example, `Square` is a subtype of `Rectangle` and can be substituted for `Rectangle` without altering the correctness of the program.

![LSP](Images/LSP.png)

### 4. **Interface Segregation Principle (ISP)**
**Definition**: Clients should not be forced to depend on interfaces they do not use.

**Explanation**: This principle advocates for creating small, specific interfaces rather than large, general-purpose ones. It ensures that a class only implements the methods it actually needs, promoting decoupling and making the system more modular.

**Example**:
```csharp
public interface IPrinter
{
    void Print();
}

public interface IScanner
{
    void Scan();
}

public class MultiFunctionPrinter : IPrinter, IScanner
{
    public void Print()
    {
        // Print implementation
    }

    public void Scan()
    {
        // Scan implementation
    }
}

public class SimplePrinter : IPrinter
{
    public void Print()
    {
        // Print implementation
    }
}
```

In this example, `IPrinter` and `IScanner` are separate interfaces, allowing `MultiFunctionPrinter` to implement both, while `SimplePrinter` only implements `IPrinter`.

![ISP](Images/ISP.png)

### 5. **Dependency Inversion Principle (DIP)**
**Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

**Explanation**: This principle encourages dependency on abstractions (e.g., interfaces or abstract classes) rather than concrete implementations. It helps achieve loose coupling between components and makes the system more flexible and easier to maintain.

**Example**:
```csharp
public interface IMessageSender
{
    void SendMessage(string message);
}

public class EmailSender : IMessageSender
{
    public void SendMessage(string message)
    {
        // Send email implementation
    }
}

public class SmsSender : IMessageSender
{
    public void SendMessage(string message)
    {
        // Send SMS implementation
    }
}

public class NotificationService
{
    private readonly IMessageSender _messageSender;

    public NotificationService(IMessageSender messageSender)
    {
        _messageSender = messageSender;
    }

    public void Notify(string message)
    {
        _messageSender.SendMessage(message);
    }
}
```
In this example, `NotificationService` depends on the abstraction `IMessageSender`, allowing it to use different implementations (`EmailSender`, `SmsSender`) without changing the `NotificationService` code.

![DI1](DI1.png)
![DI2](DI2.png)

### Summary
The SOLID principles are essential guidelines for designing robust, maintainable, and scalable software systems. 

## ***2.How dependecy***

## ***3.How dependecy***
## ***4.How dependecy***
## ***5.How dependecy***
## ***6.How dependecy***
## ***7.How dependecy***
## ***8.How dependecy***
## ***9.How dependecy***
## ***10.How dependecy***
## ***11.How dependecy***
## ***12.How dependecy***
## ***13.How dependecy***
## ***14.How dependecy***
## ***15.How dependecy***
## ***16.How dependecy***
## ***17.How dependecy***