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

In C# sind Klassen die Grundlage der objektorientierten Programmierung. Sie definieren **Datentypen mit Attributen (Felder/Eigenschaften)** und **Verhalten (Methoden)**.

### Beispiel: Basisklasse und abgeleitete Klasse

```csharp
class Animal {
    public string Name { get; set; }

    public virtual void Speak() {
        Console.WriteLine("Das Tier macht ein Geräusch");
    }
}

class Dog : Animal {
    public override void Speak() {
        Console.WriteLine("Wuff!");
    }
}

var dog = new Dog { Name = "Rex" };
dog.Speak(); // Ausgabe: Wuff!
```

### Erklärungen:

* `class` definiert eine Klasse.
* `public` macht Member von außen zugänglich.
* `virtual` erlaubt das Überschreiben der Methode.
* `override` überschreibt die geerbte Methode.
* Mit `new Dog { ... }` wird ein Objekt mit Initialisierer erzeugt.

**Hinweis:** Klassen können Konstruktoren, Properties, Felder, Methoden, Ereignisse und Indexer enthalten. Standardmäßig sind Klassen referenzbasiert.

---

## 7. Interfaces & Abstraktion

Interfaces in C# definieren einen **Vertrag**, den implementierende Klassen erfüllen müssen. Sie enthalten nur die Signaturen von Eigenschaften, Methoden, Ereignissen oder Indexern – **keine Implementierung** (außer bei Standardimplementierungen ab C# 8).

### Vorteile:

* Ermöglichen polymorphe Programmierung
* Entkoppeln Implementierung von der Schnittstelle
* Unterstützen Dependency Injection und Testbarkeit

### Beispiel: Interface und Implementierung

```csharp
interface ILogger {
    void Log(string msg);
}

class ConsoleLogger : ILogger {
    public void Log(string msg) {
        Console.WriteLine($"[LOG] {msg}");
    }
}

void Test(ILogger logger) {
    logger.Log("Dies ist eine Meldung");
}

var logger = new ConsoleLogger();
Test(logger); // Ausgabe: [LOG] Dies ist eine Meldung
```

### Erklärungen:

* `interface` deklariert eine Schnittstelle.
* Eine Klasse implementiert ein Interface mit `: InterfaceName`.
* Die Methode muss exakt zur Signatur passen.
* Du kannst mehrere Interfaces implementieren (Komma getrennt).

**Hinweis:** Interfaces fördern saubere Architekturprinzipien wie SOLID – speziell das Interface Segregation Principle und das Dependency Inversion Principle.

---

## 8. Fehlerbehandlung

Die Fehlerbehandlung in C# basiert auf dem Konzept von **Exceptions**. Diese werden bei Laufzeitfehlern ausgelöst und können mit `try` / `catch` / `finally` behandelt werden.

### Beispiel: Division durch Null

```csharp
try {
    int result = 10 / 0;
} catch (DivideByZeroException ex) {
    Console.WriteLine("Fehler: " + ex.Message);
} finally {
    Console.WriteLine("Cleanup");
}
```

### Erklärungen:

* `try`-Block enthält riskanten Code.
* `catch` fängt spezifische oder allgemeine Exceptions.
* `finally` wird **immer** ausgeführt (auch bei Fehlern oder `return`).

### Eigene Exception-Klasse

```csharp
class MyCustomException : Exception {
    public MyCustomException(string message) : base(message) {}
}
```

### Tipps:

* Fange **nie** pauschal `Exception` ohne sinnvolle Behandlung.
* Nutze gezielte `catch`-Blöcke für erwartete Fehler.
* Logge Ausnahmen zentral (z. B. mit Serilog oder ILogger).
* Werfe eigene Exceptions nur bei **wirklich außergewöhnlichen** Zuständen.

---

## 9. Dateizugriff

```csharp
File.WriteAllText("file.txt", "Hallo Datei");
string content = File.ReadAllText("file.txt");
```

---

## 10. Async & Await

Asynchrone Methoden ermöglichen **nicht-blockierende Abläufe** (z. B. Netzwerk, Datei-IO) mit dem `async`/`await`-Modell.

### Beispiel: HTTP-Request

```csharp
async Task<string> FetchAsync() {
    using HttpClient client = new();
    return await client.GetStringAsync("https://example.com");
}
```

### Erklärungen:

* `async` markiert eine Methode als asynchron.
* `await` wartet auf das Ergebnis eines Tasks.
* Rückgabetyp ist i. d. R. `Task` oder `Task<T>`.

### Wichtige Hinweise:

* Verwende `ConfigureAwait(false)` in Bibliotheken.
* Async-Methoden sollen *nicht* `void` zurückgeben (außer Eventhandler).
* Fehler in async-Methoden lösen Exceptions aus, die im `Task` verpackt sind.

### Beispiel: mehrere Tasks parallel

```csharp
var t1 = Task.Delay(1000);
var t2 = Task.Delay(1000);
await Task.WhenAll(t1, t2);
```

**Hinweis:** Vermeide `async void` – außer bei Eventhandlern.

---

## 11. LINQ Grundlagen

LINQ (Language Integrated Query) ist eine Abfragesyntax für Collections, Datenbanken, XML etc. in C#. Es kombiniert deklarativen Stil mit starker Typprüfung.

### Beispiel: Zahlen filtern

```csharp
var list = new[] {1, 2, 3, 4, 5};
var even = list.Where(x => x % 2 == 0).ToList();
```

### Erklärungen:

* `Where` ist ein **Extension-Method**, die einen Predicate-Filter anwendet.
* LINQ verwendet Lambda-Ausdrücke (`x => ...`).

### Weitere Operatoren:

```csharp
var squares = list.Select(x => x * x);
var sum = list.Sum();
var hasAnyEven = list.Any(x => x % 2 == 0);
```

### Query-Syntax (alternative Schreibweise):

```csharp
var query = from x in list
            where x % 2 == 0
            select x;
```

**Hinweis:** LINQ kann mit `IEnumerable<T>` (Lazy Evaluation) oder `IQueryable<T>` (z. B. EF Core) arbeiten.

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
C# cheatsheet
=============

C# quick reference cheat sheet that provides basic syntax and methods.

[#](#getting-started)Getting Started
------------------------------------

### [#](#hello-cs)Hello.cs

 `class Hello {   // main method   static void Main(string[] args)   {     // Output: Hello, world!     Console.WriteLine("Hello, world!");   } }`

Creates a project directory for new console application

 `$ dotnet new console`

Lists all the applications templates

 `$ dotnet new list`

Compiling and running (make sure you are in the project directory)

 `$ dotnet run Hello, world!`

### [#](#variables)Variables

 `int intNum = 9; long longNum = 9999999; float floatNum = 9.99F; double doubleNum = 99.999; decimal decimalNum = 99.9999M; char letter = 'D'; bool @bool = true; string site = "cheatsheets.zip";  var num = 999; var str = "999"; var bo = false;`

### [#](#primitive-data-types)Primitive Data Types

Data Type

Size

Range

`int`

4 bytes

\-231 to 231\-1

`long`

8 bytes

\-263 to 263\-1

`float`

4 bytes

6 to 7 decimal digits

`double`

8 bytes

15 decimal digits

`decimal`

16 bytes

28 to 29 decimal digits

`char`

2 bytes

0 to 65535

`bool`

1 bit

true / false

`string`

2 bytes per char

_N/A_

### [#](#comments)Comments

 `// Single-line comment  /* Multi-line    comment */  // TODO: Adds comment to a task list in Visual Studio  /// Single-line comment used for documentation  /** Multi-line comment     used for documentation **/`

### [#](#strings)Strings

 `string first = "John"; string last = "Doe";  // string concatenation string name = first + " " + last; Console.WriteLine(name); // => John Doe`

See: [Strings](#c-strings)

### [#](#user-input)User Input

 `Console.WriteLine("Enter number:"); if(int.TryParse(Console.ReadLine(),out int input)) {   // Input validated   Console.WriteLine($"You entered {input}"); }`

### [#](#conditionals)Conditionals

 `int j = 10;  if (j == 10) {   Console.WriteLine("I get printed"); } else if (j > 10) {   Console.WriteLine("I don't"); } else {   Console.WriteLine("I also don't"); }`

### [#](#arrays)Arrays

 `char[] chars = new char[10]; chars[0] = 'a'; chars[1] = 'b';  string[] letters = {"A", "B", "C"}; int[] mylist = {100, 200}; bool[] answers = {true, false};`

### [#](#loops)Loops

 `int[] numbers = {1, 2, 3, 4, 5};  for(int i = 0; i < numbers.Length; i++) {   Console.WriteLine(numbers[i]); }`

* * *

 `foreach(int num in numbers) {   Console.WriteLine(num); }`

[#](#c-strings)C# Strings
-------------------------

### [#](#string-concatenation)String concatenation

 `string first = "John"; string last = "Doe";  string name = first + " " + last; Console.WriteLine(name); // => John Doe`

### [#](#string-interpolation)String interpolation

 `string first = "John"; string last = "Doe";  string name = $"{first} {last}"; Console.WriteLine(name); // => John Doe`

### [#](#string-members)String Members

Member

Description

Length

A property that returns the length of the string.

Compare()

A static method that compares two strings.

Contains()

Determines if the string contains a specific substring.

Equals()

Determines if the two strings have the same character data.

Format()

Formats a string via the {0} notation and by using other primitives.

Trim()

Removes all instances of specific characters from trailing and leading characters. Defaults to removing leading and trailing spaces.

Split()

Removes the provided character and creates an array out of the remaining characters on either side.

### [#](#verbatim-strings)Verbatim strings

 `string longString = @"I can type any characters in here !#@$%^&*()__+ '' \n \t except double quotes and I will be taken literally. I even work with multiple lines.";`

### [#](#member-example)Member Example

 `// Using property of System.String string lengthOfString = "How long?"; lengthOfString.Length           // => 9  // Using methods of System.String lengthOfString.Contains("How"); // => true`

[#](#misc)Misc
--------------

### [#](#general-net-terms)General .NET Terms

Term

Definition

Runtime

A collection of services that are required to execute a given compiled unit of code.

Common Language Runtime (CLR)

Primarily locates, loads, and managed .NET objects. The CLR also handles memory management, application hosting, coordination of threads, performing security checks, and other low-level details.

Managed code

Code that compiles and runs on .NET runtime. C#/F#/VB are examples.

Unmanaged code

Code that compiles straight to machine code and cannot be directly hosted by the .NET runtime. Contains no free memory management, garbage collection, etc. DLLs created from C/C++ are examples.
