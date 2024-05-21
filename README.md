
<p align="center">
  <img src = "https://miro.medium.com/max/2728/1*7I6oONv2fGLQJcNEFA4QSw.png" width=800 height=300>
</p>

## 🚩 Table of Contents

- [Programcs](#programcs)
- [Dependency Injection](dependency-injection)
- [Middleware](middleware)

## Programcs
### Role of Program.cs:
Program.cs serves as the entry point for your ASP.NET Core application.
It orchestrates the startup process, configures the web host, and sets up services.

### Main Method:
In previous versions (like .NET 5 and Core), you defined a Main() method in Program.cs.
This method was responsible for bootstrapping the application.

Example of a typical Main() method:

```csharp
public static void Main(string[] args)
{
    CreateHostBuilder(args).Build().Run();
}
```

### Startup.cs vs. Program.cs:
In the past, Startup.cs handled service registration and HTTP request pipeline configuration.
With ASP.NET 6, Startup.cs is no longer mandatory.
You can now combine the responsibilities of both files in Program.cs.

### Dependency Injection and Middleware:
Register services with the dependency injection container in Program.cs.
Define how your application responds to incoming HTTP requests using middleware.
The ConfigureServices() method registers services, and Configure() sets up the HTTP pipeline.

**Example:**

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorPages();
builder.Services.AddControllersWithViews();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseAuthorization();

app.MapGet("/hi", () => "Hello!");

app.MapDefaultControllerRoute();
app.MapRazorPages();

app.Run();
```


---












## Dependency Injection
### What is Dependency Injection?
Dependency injection aims to separate the creation and use of object dependencies.
Instead of classes creating their own dependencies directly, dependencies are provided from an external source (the injector).
Client classes expose their dependencies via constructor parameters, method parameters, or public properties.
The injector handles dependency creation and management, promoting loose coupling between classes and their dependencies1.

### Benefits of DI:
- Loose Coupling: Classes don’t directly create dependencies; they receive them from an injector.
- Easy Testing: Swapping real dependencies with mock objects for testing is straightforward.
- Switching Implementations: Changing dependency implementations doesn’t require modifying client classes.
- Code Reuse: Common dependencies can be shared across multiple classes via the injector.
- Configuration through Code: Dependency configuration is specified in code, making it easier to track2.

### Using the Built-In .NET Core DI Container:
Install the Microsoft.Extensions.DependencyInjection NuGet package.
Define service interfaces and their implementations.
Register services in the program.cs ConfigureServices method.
Inject services into constructors where needed.
The framework manages dependency instantiation and disposal

### Constructor Injection:

```csharp
public class UserService
{
    private readonly IUserRepository _userRepository;

    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    // Other methods and logic...
}

```

### ASP.NET Core

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllersWithViews();
var app = builder.Build();

// Configure middleware, routes, etc.

app.Run();
```

--- 










## Middleware

### What is Middleware?
Middleware is software assembled into an app pipeline to handle requests and responses.
It runs between the client and server, intercepting and processing HTTP requests.

Each middleware component can:
- Pass the request to the next component.
- Perform work before and after the next component.
- Short-circuit the pipeline if needed.

### Request Pipeline in ASP.NET Core:
- The ASP.NET Core request pipeline consists of a sequence of request delegates.
- Each delegate handles an HTTP request.
- Middleware components are request delegates configured using Run, Map, and Use extension methods.
- Exception-handling middleware should be called early in the pipeline.

### Creating Custom Middleware:
You can write custom middleware by:
- Defining it in a class with an InvokeAsync method.
- Registering it in the Program.cs using app.UseMiddleware<YourMiddleware>().
Middleware components allow you to manipulate requests and responses, log data, and more.

```csharp

```
<p align="center">
<img src = "https://wat-user-images.s3.amazonaws.com/1634560374197%2FN6hj9g0G6p%2FScreenshot_(324).png" width=700 height=400>
</p>
