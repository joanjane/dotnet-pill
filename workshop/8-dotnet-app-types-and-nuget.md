## 8. .NET Application Types and NuGet packages

### Solution files

In .NET, it's common to create a Solution File (.sln) and then add projects to the solution. Each project may have different types, like ASPNET Core APIs, Console apps, class libraries...

You may start creating a solution with:

```
dotnet new sln -n PillSamples
```

### Console applications

- For building utilities or background processes.
- With recent language feature using top level statements, program may look like this

Program.cs example using Program class (traditional approach):

```csharp
class Program
{
    static void Main()
    {
        Console.WriteLine("Hello, Console Application!");
    }
}
```

Program.cs example using top level statements (newer approach):

```csharp
// This does not require any boilerplate like creating a Program class.
Console.WriteLine("Hello, Console Application!");
```

The .csproj file will look like this:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>
</Project>
```

#### Create a Console app with CLI:

You may start creating a .NET 8.0 console app with:

```
dotnet new console -n PillSamples.ConsoleApp -f net8.0 -o src/PillSamples.ConsoleApp
```

Then add the project to the solution with:

```
dotnet sln PillSamples.sln add ./src/PillSamples.ConsoleApp
```

Run the application:

Then add the project to the solution with:

```
dotnet run --project ./src/PillSamples.ConsoleApp
```

### ASP.NET Core

- Suitable for building websites, RESTful APIs...
- Cross-platform and high-performance.
- Supports MVC (Model-View-Controller) and Razor Pages.

Example using minimal APIs:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello, ASP.NET Core!");

app.Run();
```

The .csproj file will look like this:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
	<PropertyGroup>
		<TargetFramework>net8.0</TargetFramework>
	</PropertyGroup>
</Project>
```

#### Create a Controller based API with CLI:

You may start creating a ASP.NET 8.0 API with:

```
dotnet new webapi -n PillSamples.WebApi -f net8.0 -o src/PillSamples.WebApi -controllers
```

Then add the project to the solution with:

```
dotnet sln PillSamples.sln add ./src/PillSamples.WebApi
```

Run the application:

```
dotnet run --project ./src/PillSamples.WebApi
```

You can use http://localhost:{YOURPORT}/swagger/index.html or the .http file generated to test your API.

### Class libraries

- Reusable code components packaged as DLLs.
- Can be referenced by other .NET applications.
- Useful for sharing logic, utilities, or models.

The .csproj file will look like this:

```xml
<Project Sdk="Microsoft.NET.Sdk">
</Project>
```

#### Create a class library with CLI:

You may start creating a .NET 8.0 api app with:

```
dotnet new classlib -n PillSamples.Library -f net8.0 -o src/PillSamples.Library
```

Then add the project to the solution with:

```
dotnet sln PillSamples.sln add ./src/PillSamples.Library
```

Then add the library on another project for testing:

```
dotnet reference add ./src/PillSamples.Library --project ./src/PillSamples.WebApi
```

### Using NuGet packages

- Install packages in the project easily
- Avoid installing different package versions between projects of the same solution.
- In certain scenarios, a `Directory.Packages.props` file defined at root may centralize which package versions to use.

The .csproj file will look like this after installing a NuGet package serializing Json:

```xml
<Project Sdk="Microsoft.NET.Sdk">
	<ItemGroup>
        <PackageReference Include="System.Text.Json" Version="8.0.6" />
    </ItemGroup>
</Project>
```

#### Add a package using the CLI:

```
dotnet add package System.Text.Json --version 8.0.6 --project ./src/PillSamples.Library
```

### Test projects

You can create a test project with XUnit, one of the most common testing libraries in .NET.

```
dotnet new xunit -n PillSamples.Tests -f net8.0 -o test/PillSamples.Tests
```

Then add the project to the solution:

```
dotnet sln PillSamples.sln add ./test/PillSamples.Tests
```

Finally, you can reference the Library project to add some tests:

```
dotnet reference add ./src/PillSamples.Library --project ./test/PillSamples.Tests
```

## Create the solution at once

Delete all the previously created code (if needed):

- bash: `rm *.sln && rm -rf ./src && rm -rf ./test`
- pwsh: `Remove-Item *.sln && Remove-Item ./src -Recurse -Force && Remove-Item ./test -Recurse -Force`

Create the full solution from previous steps:

```bash
dotnet new sln -n PillSamples
dotnet new console -n PillSamples.ConsoleApp -f net8.0 -o src/PillSamples.ConsoleApp
dotnet sln PillSamples.sln add ./src/PillSamples.ConsoleApp
dotnet new webapi -n PillSamples.WebApi -f net8.0 -o src/PillSamples.WebApi -controllers
dotnet sln PillSamples.sln add ./src/PillSamples.WebApi
dotnet new classlib -n PillSamples.Library -f net8.0 -o src/PillSamples.Library
dotnet sln PillSamples.sln add ./src/PillSamples.Library
dotnet reference add ./src/PillSamples.Library --project ./src/PillSamples.WebApi
dotnet add package System.Text.Json --version 8.0.6 --project ./src/PillSamples.Library
dotnet new xunit -n PillSamples.Tests -f net8.0 -o test/PillSamples.Tests
dotnet sln PillSamples.sln add ./test/PillSamples.Tests
dotnet reference add ./src/PillSamples.Library --project ./test/PillSamples.Tests
```
