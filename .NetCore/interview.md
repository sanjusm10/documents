## ***1.Scoped, Singleton and Transient difference and usage***

In C#, dependency injection (DI) is a powerful design pattern used to achieve Inversion of Control (IoC) between classes and their dependencies. The .NET Core DI container provides three main lifetimes for services: Scoped, Singleton, and Transient. Each has different use cases and behaviors. Here's a detailed comparison and usage examples:

### **1. Scoped**

**Definition**: Scoped services are created once per request (or scope). They are shared within the same request but not across different requests.

**Use Case**: Scoped services are ideal for situations where you want to maintain state within a single request but do not want to share that state across different requests. Common use cases include database contexts in web applications.

**Example**:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<IMyService, MyService>();
}

public class MyService : IMyService
{
    // Implementation of the service
}
```

**Usage**:

```csharp
public class MyController : Controller
{
    private readonly IMyService _myService;

    public MyController(IMyService myService)
    {
        _myService = myService;
    }

    public IActionResult Index()
    {
        // Use the scoped service
        return View();
    }
}
```

In this example, `MyService` is registered as a scoped service, meaning a new instance of `MyService` will be created for each HTTP request and shared within that request.

### **2. Singleton**

**Definition**: Singleton services are created once and shared across the entire application. A single instance is used for all requests and throughout the application's lifetime.

**Use Case**: Singleton services are ideal for shared, read-only data or expensive-to-create services that should be shared across the entire application. Examples include configuration settings and services that manage shared resources.

**Example**:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IMyService, MyService>();
}

public class MyService : IMyService
{
    // Implementation of the service
}
```

**Usage**:

```csharp
public class MyController : Controller
{
    private readonly IMyService _myService;

    public MyController(IMyService myService)
    {
        _myService = myService;
    }

    public IActionResult Index()
    {
        // Use the singleton service
        return View();
    }
}
```

In this example, `MyService` is registered as a singleton, meaning the same instance of `MyService` will be used across all requests throughout the application's lifetime.

### **3. Transient**

**Definition**: Transient services are created each time they are requested. They are not shared between requests or instances.

**Use Case**: Transient services are ideal for lightweight, stateless services that do not maintain any state between different requests or consumers. Examples include utility services and lightweight operations.

**Example**:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<IMyService, MyService>();
}

public class MyService : IMyService
{
    // Implementation of the service
}
```

**Usage**:

```csharp
public class MyController : Controller
{
    private readonly IMyService _myService;

    public MyController(IMyService myService)
    {
        _myService = myService;
    }

    public IActionResult Index()
    {
        // Use the transient service
        return View();
    }
}
```

In this example, `MyService` is registered as a transient service, meaning a new instance of `MyService` will be created each time it is requested.

### **Key Differences**

| Feature          | Scoped                                  | Singleton                                    | Transient                                |
|------------------|-----------------------------------------|---------------------------------------------|-----------------------------------------|
| **Lifetime**     | Once per request                        | Once per application lifetime               | Each time they are requested            |
| **Scope**        | Shared within the same request          | Shared across the entire application        | Not shared, new instance each time      |
| **Use Case**     | Maintaining state within a request      | Shared data or expensive-to-create services | Lightweight, stateless services         |
| **Example**      | Database context                        | Configuration settings                      | Utility services                        |

### **Summary**

- **Scoped**: Ideal for services that need to maintain state within a single request, such as database contexts.
- **Singleton**: Ideal for shared, read-only data or services that should be shared across the entire application, such as configuration settings.
- **Transient**: Ideal for lightweight, stateless services that are created and destroyed quickly, such as utility services.

By understanding these differences, you can choose the appropriate service lifetime for your specific needs, ensuring optimal performance and behavior in your applications.
