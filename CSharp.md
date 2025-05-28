# 📘 C# .NET Cheat Sheet

## Inhaltsverzeichnis

1. [Einführung](#1-einf%C3%BChrung)
2. [Grundlegende Syntax](#2-grundlegende-syntax)
3. [Datentypen & Variablen](#3-datentypen--variablen)
4. [Kontrollstrukturen](#4-kontrollstrukturen)
5. [Methoden & Parameter](#5-methoden--parameter)
6. [OOP: Klassen & Vererbung](#6-oop-klassen--vererbung)
7. [Interfaces & Abstraktion](#7-interfaces--abstraktion)
8. [Fehlerbehandlung](#8-fehlerbehandlung)
9. [Dateizugriff](#9-dateizugriff)
10. [Async & Await](#10-async--await)
11. [LINQ Grundlagen](#11-linq-grundlagen)
12. [Entity Framework Core](#12-entity-framework-core)
13. [ASP.NET Core Minimal API](#13-aspnet-core-minimal-api)

---

## 1. Einführung

C# ist eine objektorientierte Sprache für die .NET-Plattform mit starkem Typensystem, hoher Sicherheit und modernem Feature-Set.

---

## 2. Grundlegende Syntax

```csharp
using System;

namespace MyApp {
    class Program {
        static void Main(string[] args) {
            Console.WriteLine("Hello, World!");
        }
    }
}
```

---

## 3. Datentypen & Variablen

```csharp
int a = 5;
double pi = 3.14;
bool isDone = false;
char c = 'X';
string message = "Hallo";
var inferred = 42;
```

---

## 4. Kontrollstrukturen

```csharp
if (a > 0) {
    Console.WriteLine("Positiv");
} else {
    Console.WriteLine("Nicht positiv");
}

for (int i = 0; i < 5; i++) {
    Console.WriteLine(i);
}
```

---

## 5. Methoden & Parameter

```csharp
int Add(int x, int y) => x + y;

void Print(string msg = "Default") {
    Console.WriteLine(msg);
}
```

---

## 6. OOP: Klassen & Vererbung

```csharp
class Animal {
    public virtual void Speak() => Console.WriteLine("Sound");
}

class Dog : Animal {
    public override void Speak() => Console.WriteLine("Bark");
}
```

---

## 7. Interfaces & Abstraktion

```csharp
interface ILogger {
    void Log(string msg);
}

class ConsoleLogger : ILogger {
    public void Log(string msg) => Console.WriteLine(msg);
}
```

---

## 8. Fehlerbehandlung

```csharp
try {
    int result = 10 / 0;
} catch (DivideByZeroException ex) {
    Console.WriteLine("Fehler: " + ex.Message);
} finally {
    Console.WriteLine("Cleanup");
}
```

---

## 9. Dateizugriff

```csharp
File.WriteAllText("file.txt", "Hallo Datei");
string content = File.ReadAllText("file.txt");
```

---

## 10. Async & Await

```csharp
async Task<string> FetchAsync() {
    using HttpClient client = new();
    return await client.GetStringAsync("https://example.com");
}
```

---

## 11. LINQ Grundlagen

```csharp
var list = new[] {1, 2, 3, 4};
var even = list.Where(x => x % 2 == 0).ToList();
```

---

## 12. Entity Framework Core

```csharp
class AppDbContext : DbContext {
    public DbSet<User> Users { get; set; }
}

class User {
    public int Id { get; set; }
    public string Name { get; set; }
}
```

---

## 13. ASP.NET Core Minimal API

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello API");

app.Run();
```

---

*Stand: .NET 8 – für produktionsreife Anwendungen empfohlene Praxis.*
