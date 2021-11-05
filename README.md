# Minimal Api Nonsense

> *visual studio code (with PATH integration) and dotnet version 6 required.*

- [.NET 6](https://dotnet.microsoft.com/download/dotnet/6.0)
- [Visual Studio Code](https://code.visualstudio.com/download)


## Project Setup in Shell

> works in powershell, zsh, or bash
```bash
mkdir MinimalApiNonsense && cd MinimalApiNonsense && dotnet new web && code .
```


## Make A Whole API And A Client That Calls Itself For No Reason In 35 lines of C#

1. copy and paste the following code into Program.cs

```csharp
using System.Text.Json;
using System.Net.Http;
using System.Threading.Tasks;
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();
app.MapGet("/", () => "Hello World!");
app.MapGet("/object", () =>
  JsonSerializer.Serialize(new
  {
    Thing = 1,
    Hello = "World"
  })
);
app.MapGet("/matching/{id}", (int id) => JsonSerializer.Serialize(id));
var client = new HttpClient();
async Task RunIt()
{
  await Task.Run(() => Thread.Sleep(1000));
  if (client == null || client == default) return;
  var root = await (await (client.GetAsync("http://localhost:3000/"))).Content.ReadAsStringAsync();
  var obj = await (await (client.GetAsync("http://localhost:3000/object"))).Content.ReadAsStringAsync();
  var match = await (await (client.GetAsync("http://localhost:3000/matching/21"))).Content.ReadAsStringAsync();
  Console.WriteLine($"{obj}");
  Console.WriteLine($"{root}");
  Console.WriteLine($"{match}");
}
var clientOperations = RunIt();
app.Run("http://localhost:3000");
await clientOperations;
```


## Run the thing...

```
dotnet run
```

`¯\_(ツ)_/¯`
