# Minimal Api Nonsense

> *visual studio code (with PATH integration), dotnet version 6, and dotnet cli required*

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
app.MapGet("/", () => "Hello! World");
app.MapGet("/object", () =>
  JsonSerializer.Serialize(new
  {
    Thing = 1,
    Hello = "World"
  })
);
app.MapGet("/matching/{id}", (int id) => JsonSerializer.Serialize(id));
var client = new HttpClient();
async Task CallThatShit()
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
var clientOperations = CallThatShit();
app.Run("http://localhost:3000");
await clientOperations;
```


## Run the thing...

> works in powershell, zsh, or bash... *you know what? I'm just going to say it this time, it runs where ever you damn well please.*
```
dotnet run
```

`¯\_(ツ)_/¯`
