# Options Pattern in .NET

## Overview
The Options Pattern provides a mechanism for creating strongly-typed access to groups of related settings, eliminating "magic strings" and improving type safety in configuration management.

```mermaid
graph LR
    A[Configuration] --> B[Options Classes]
    B --> C[Strongly Typed Settings]
    C --> D[Type-Safe Access]
    
    style A fill:#f9d,stroke:#333
    style B fill:#afd,stroke:#333
    style C fill:#daf,stroke:#333
    style D fill:#fda,stroke:#333
```

## Key Benefits

| Benefit | Description |
|---------|-------------|
| Type Safety | Compile-time checking of configuration access |
| Encapsulation | Settings grouped by functionality |
| Separation of Concerns | Each configuration group isolated in its own class |
| IntelliSense Support | IDE assistance for property access |

## Options Class Rules

1. **Class Requirements**
   - Must not be abstract
   - Must have public properties
   - Properties must be read/write
   - Property types must match configuration values

2. **Example Configuration**
   ```json
   {
     "Jwt": {
       "Key": "",
       "Issuer": "SurveyBasketApp",
       "Audience": "SurveyBasketApp users",
       "ExpiryMinutes": 30
     }
   }
   ```

3. **Corresponding Options Class**
   ```csharp
   public class JwtOptions
   {
       public string Key { get; set; } = string.Empty;
       public string Issuer { get; set; } = string.Empty;
       public string Audience { get; set; } = string.Empty;
       public int ExpiryMinutes { get; set; }
   }
   ```

## Before vs After Options Pattern

### Before (Using Direct Configuration)
```csharp
public class Service
{
    private readonly IConfiguration _configuration;
    
    public Service(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public void DoSomething()
    {
        // Magic strings - prone to errors
        var issuer = _configuration["Jwt:Issuer"];
        var expiryMinutes = int.Parse(_configuration["Jwt:ExpiryMinutes"]);
    }
}
```

### After (Using Options Pattern)
```csharp
public class Service
{
    private readonly JwtOptions _jwtOptions;
    
    public Service(IOptions<JwtOptions> jwtOptions)
    {
        _jwtOptions = jwtOptions.Value;
    }

    public void DoSomething()
    {
        // Strongly typed access
        var issuer = _jwtOptions.Issuer;
        var expiryMinutes = _jwtOptions.ExpiryMinutes;
    }
}
```

## Common Use Cases

| Scenario | Example Settings |
|----------|-----------------|
| Authentication | JWT Configuration |
| Email Service | SMTP Settings |
| Logging | Log Levels and Paths |
| API Clients | Endpoints and Timeouts |

## Benefits of Options Pattern

1. **Type Safety**
   - Compile-time error checking
   - No magic strings
   - Proper type conversion

2. **Organization**
   - Logically grouped settings
   - Clear separation of concerns
   - Better maintainability

3. **Development Experience**
   - IntelliSense support
   - Easier refactoring
   - Better discoverability

## Best Practices

1. **Naming Conventions**
   - Suffix classes with "Options"
   - Match configuration section names
   - Use meaningful property names

2. **Validation**
   - Add data annotations
   - Implement validation logic
   - Handle missing values gracefully

3. **Documentation**
   - Document required settings
   - Provide example configurations
   - Explain validation rules

## Implementation Steps

1. **Create Options Class**
   - Define properties matching configuration
   - Add appropriate data types
   - Include any necessary validation

2. **Register with DI**
   - Configure in Program.cs/Startup.cs
   - Bind to configuration section
   - Set lifetime scope

3. **Inject and Use**
   - Inject IOptions<T> in constructors
   - Access typed configuration
   - Use in services/controllers

---

The Options Pattern is a powerful feature in .NET that promotes clean, maintainable, and type-safe configuration access. It's particularly useful for large applications with multiple configuration groups.
