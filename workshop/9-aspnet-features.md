## 9. ASP.NET features

Here there are highlighted some key features of the framework.

### Dependency Injection

.NET implements [DI](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection) in a simple configuration. You can define the DI for your classes as below. It's quite common to create Interfaces and Classes to avoid coupling of concrete implementations following the SOLID principles.

```csharp
// New instance every time is injected
builder.Services.AddTransient<IMyTransientService, MyTransientService>();
// Same instance in the scoped service provider. You need to create an scope first.
builder.Services.AddScoped<IMyScopedService, MyScopedService>();
// Single instance
builder.Services.AddSingleton<IMySingletonService, MySingletonService>();

// You don't need an interface, you can inject a class too
builder.Services.AddTransient<MyServiceWithoutInterface>();
```

### Configuration

.NET implements the [IConfiguration](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/) interface, which has many configuration providers implemented by default. The values are defined following the order from the first to the last that defines the variable.

1. Command-line arguments using the Command-line configuration provider.
2. Non-prefixed environment variables using the Non-prefixed environment variables configuration provider.
3. User secrets when the app runs in the Development environment.
4. appsettings.{Environment}.json using the JSON configuration provider. For example, appsettings. Production.json and appsettings.Development.json.
5. appsettings.json using the JSON configuration provider.

You can then inject an IConfiguration object and get your configured values as `Configuration["MySection:MyKey"]`.

It's pretty common that instead of injecting IConfiguration, you use the Options pattern, as defined in the [docs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options)

### Logging

You can control the verbosity of the logs by namespace/component, easily configured in appsettings. In the Program.cs, you can add more [Logging providers](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/logging/) that implement the logging interface.

```jsonc
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "PillSamples": "Debug",
      "PillSamples.WebApi": "Information"
    }
  }
```

You can inject the logger to your component with:

```cs
public class YourClass
{
    private readonly ILogger<YourClass> _logger;
    public YourClass(ILogger<YourClass> logger)
    {
        _logger = logger;
        _logger.LogInformation("Today is {DayOfWeek}", DateTime.DayOfWeek);
    }
}
```

### Middlewares

- Middleware components are classes with a constructor accepting RequestDelegate and an Invoke or InvokeAsync method.
- Middleware can inspect, modify, or short-circuit HTTP requests and responses.
- Middleware is registered in the request pipeline in the order you want them to execute.
- Low level approach, there are some specialized middlewares in the framework, such as [ExceptionHandlers](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/error-handling), Authorization...

Read the [docs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware).

### Authorization policies

You can define policies that require specific criteria in order to fulfill the authorization rules. Then you can decorate one endpoint with a specific policy, so you can control access in a simple way. Read more [here](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/policies)
