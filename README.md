# Endpoints

`ChrisMavrommatis.Endpoints` is a .NET Package aimed to provide the Endpoint pattern in your projects.

The project aims to provide with utilities that will make your API structure look like this
```yml
Endpoints
 - Users
    - Get.cs
    - List.cs
    - Create.cs
    - Delete.cs
  - Products
    - Get.cs
    - List.cs
    - Create.cs
    - Delete.cs
```

## Installation

Install the package via NuGet Package Manager
```bash
Install-Package ChrisMavrommatis.Endpoints
```

Or via .NET CLI:
```bash
dotnet add package ChrisMavrommatis.Endpoints
```

## Usage
The library masks the `ControllerBase` class in several layers ending up with the following classes:

- `EndpointWithoutRequest`
- `EndpointWithRequest<TRequest>`

There is an additional option to define the namespace of the endpoint by using the `[Route]` attribute. 
The `[namespace]` placeholder will be replaced with the namespace of the endpoint.

For example the `List.cs` file would look like this:
```csharp
namespace MyProject.Endpoints.Users;

[Route("api/[namespace]")]
public class List : EndpointWithoutRequest
{
  
  // GET: api/Users
  [HttpGet]
  public override Task<IActionResult> HandleAsync()
  {
    // Code ...
  }
}
```

The `Get.cs` file would look like this:
```csharp
namespace MyProject.Endpoints.Users;

[Route("api/[namespace]")]
public class Get : EndpointWithRequest<GetUserRequest>
{
  
  // GET: api/Users/{id}
  [HttpGet("{id}")]
  public override Task<IActionResult> HandleAsync(GetUserRequest request)
  {
    // Code ...
  }
}
```

The `Create.cs` file would look like this:
```csharp
namespace MyProject.Endpoints.Users;

[Route("api/[namespace]")]
public class Create : EndpointWithRequest<CreateUserRequest>
{
  
  // POST: api/Users
  [HttpPost]
  public override Task<IActionResult> HandleAsync(CreateUserRequest request)
  {
    // Code ...
  }
}
```

In order to use the Namespace route you need to use the `UseNamespaceRouteToken` extension method while adding controllers
```csharp
builder.Services.AddControllers(options =>
{
  options.UseNamespaceRouteToken();
})
```

And while configuring the `SwaggerGenOptions` use the `TagActionsByEndpointNamespaceOrDefault` extension method
```csharp
builder.Services.AddSwaggerGen(options =>
{
  options.TagActionsByEndpointNamespaceOrDefault();
})
```


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

