# 📘 C# .NET Cheat Sheet

## Inhaltsverzeichnis

1. [Einführung](#1-einführung)
2. [Grundlegende Syntax](#2-grundlegende-syntax)
3. [Datentypen & Variablen](#3-datentypen--variablen)

   * [Primitive Typen](#primitive-datentypen)
   * [var-Schlüsselwort](#var-schlüsselwort)
   * [Properties](#properties-eigenschaften)
   * [Nullable-Typen](#nullable-typen)
   * [Konstanten und readonly](#konstanten-und-readonly)
   * [Datenbindung](#datenbindung-binding)
   * [Binding in Blazor](#binding-in-blazor)
   * [Command Binding (MVVM)](#command-binding-in-mvvm-z-b-wpf)
   * [Validierung mit DataAnnotations](#validierung-mit-dataannotations)
   * [ObservableCollection](#observablecollectiont)
   * [BindingContext](#bindingcontext-z-b-in-maui--xamarin)
   * [Benutzerdefinierte Validierung](#benutzerdefinierte-validierungsattribute)
   * [ValidationResult verwenden](#verwendung-von-validationresult)
4. [Kontrollstrukturen](#4-kontrollstrukturen)
5. [Methoden & Parameter](#5-methoden--parameter)
6. [OOP: Klassen & Vererbung](#6-oop-klassen--vererbung)

   * [Vererbung und Polymorphie](#beispiel-basisklasse-und-abgeleitete-klasse)
   * [Virtuelle und überschriebene Methoden](#erklärungen)
7. [Interfaces & Abstraktion](#7-interfaces--abstraktion)

   * [Beispielimplementierung](#beispiel-interface-und-implementierung)
   * [Designprinzipien](#erklärungen)
8. [Fehlerbehandlung](#8-fehlerbehandlung)

   * [try-catch-finally](#beispiel-division-durch-null)
   * [Eigene Exception-Klasse](#eigene-exception-klasse)
   * [Best Practices](#tipps)
9. [Dateizugriff](#9-dateizugriff)
10. [Async & Await](#10-async--await)

    * [Grundlagen](#erklärungen)
    * [Parallele Tasks](#beispiel-mehrere-tasks-parallel)
11. [LINQ Grundlagen](#11-linq-grundlagen)

    * [Method Syntax](#weitere-operatoren)
    * [Query Syntax](#query-syntax-alternative-schreibweise)
12. [Entity Framework Core](#12-entity-framework-core)

    * [Installation](#installation-per-cli)
    * [Modell & DbContext](#modell--dbcontext)
    * [Migrationen](#migration--datenbank-erzeugen)
    * [CRUD](#crud-operationen)
13. [ASP.NET Core Minimal API](#13-aspnet-core-minimal-api)

---

## Anhang: Technische Begriffe

### .NET Runtime

Ein Satz von Diensten, der zum Ausführen von .NET-Anwendungen erforderlich ist. Er umfasst u. a.:

* Speicherverwaltung
* Assembly- und Typauflösung
* Exception-Handling
* JIT-Kompilierung

### Common Language Runtime (CLR)

Kernkomponente der .NET-Runtime, die:

* Code lädt und ausführt
* Garbage Collection durchführt
* Sicherheitsüberprüfungen und Exception-Handling steuert

### Managed Code

Code, der vom CLR verwaltet wird. Er profitiert von Speicherverwaltung, Sicherheit, Portabilität und Typüberprüfung.

### Unmanaged Code

Code, der direkt vom Betriebssystem ausgeführt wird (z. B. C/C++ DLLs). Keine Garbage Collection oder CLR-Dienste.

### Assembly

Die kleinste deploybare Einheit einer .NET-Anwendung (.dll oder .exe). Enthält IL (Intermediate Language), Metadaten und Ressourcen.

### IL (Intermediate Language)

Von C#-Compiler erzeugter Zwischencode, der zur Laufzeit vom CLR (JIT) in Maschinencode übersetzt wird.

### NuGet

Paketmanager für .NET, vergleichbar mit npm (Node.js) oder pip (Python).

```bash
# Beispiel: Newtonsoft.Json installieren
$ dotnet add package Newtonsoft.Json
```

### Garbage Collector (GC)

Automatischer Speicherbereiniger. Entfernt nicht mehr referenzierte Objekte, um Speicher freizugeben.

### Just-in-Time Compilation (JIT)

Kompiliert IL-Code zur Laufzeit in nativen Maschinencode, wenn dieser benötigt wird.

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

### Datenbindung (Binding)

Datenbindung bezeichnet die automatische Synchronisierung von Daten zwischen UI-Komponenten (z. B. in WPF, WinForms, Blazor) und Datenquellen (z. B. Models).

Es gibt verschiedene Bindungsarten:

* **One-Way Binding:** Daten fließen vom Model zur View
* **Two-Way Binding:** Daten fließen in beide Richtungen (z. B. bei Formulareingaben)
* **One-Time Binding:** Nur beim Initialisieren gebunden

### Beispiel in WPF (XAML):

```xml
<TextBox Text="{Binding UserName, Mode=TwoWay}" />
```

### Beispiel: ViewModel in C\#

```csharp
public class UserViewModel : INotifyPropertyChanged {
    private string _userName;

    public string UserName {
        get => _userName;
        set {
            _userName = value;
            OnPropertyChanged();
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```

### Erklärung:

* `INotifyPropertyChanged` signalisiert der UI, wenn sich ein Property-Wert ändert.
* `PropertyChanged` wird durch `OnPropertyChanged` ausgelöst.
* `CallerMemberName` sorgt dafür, dass der Propertyname automatisch übergeben wird.

### Binding in Blazor

In Blazor wird Datenbindung über `@bind` direkt in HTML-artigem Razor-Syntax umgesetzt.

```razor
<input @bind="UserName" />
```

* `@bind` erstellt eine Two-Way-Bindung zwischen UI und Property.
* Intern werden `value`- und `onchange`-Events gekoppelt.

### Command Binding in MVVM (z. B. WPF)

Für Aktionen wird in WPF typischerweise das `ICommand`-Interface verwendet:

```csharp
public class RelayCommand : ICommand {
    private readonly Action _execute;
    public RelayCommand(Action execute) => _execute = execute;

    public event EventHandler CanExecuteChanged;
    public bool CanExecute(object parameter) => true;
    public void Execute(object parameter) => _execute();
}
```

```csharp
public ICommand SubmitCommand { get; }
SubmitCommand = new RelayCommand(() => Submit());
```

```xml
<Button Content="OK" Command="{Binding SubmitCommand}" />
```

### Validierung mit DataAnnotations

### ObservableCollection<T>

In WPF und MAUI wird zur dynamischen Listenbindung oft `ObservableCollection<T>` verwendet. Sie informiert die UI über Änderungen (Add/Remove/Reset).

```csharp
public ObservableCollection<string> Items { get; } = new();
Items.Add("Element 1");
```

### BindingContext (z. B. in MAUI / Xamarin)

```csharp
this.BindingContext = new MyViewModel();
```

Damit wird das ViewModel global für Bindings innerhalb der View (z. B. Page, UserControl) gesetzt.

### Benutzerdefinierte Validierungsattribute

```csharp
public class NotEmptyAttribute : ValidationAttribute {
    public override bool IsValid(object value) =>
        value is string str && !string.IsNullOrWhiteSpace(str);
}

public class ContactForm {
    [NotEmpty(ErrorMessage = "Name darf nicht leer sein")]
    public string Name { get; set; }
}
```

### Verwendung von ValidationResult

```csharp
var context = new ValidationContext(model);
var results = new List<ValidationResult>();
bool isValid = Validator.TryValidateObject(model, context, results, true);
```

* Mit `Validator.TryValidateObject` kann man manuell ein Objekt gegen Regeln prüfen.
* `ValidationResult` enthält dabei Details zu jedem Verstoß.

```csharp
public class LoginModel {
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [MinLength(6)]
    public string Password { get; set; }
}
```

* DataAnnotation-Attribute validieren Eingaben automatisch.
* In ASP.NET und Blazor ist die Integration direkt unterstützt.

**Hinweis:** In Blazor, WPF und MAUI erfolgt Bindung in Kombination mit PropertyChanged-Logik, Commands und Validierungsattributen.

C# unterscheidet zwischen **Werttypen** (Value Types) und **Referenztypen** (Reference Types):

* **Werttypen** speichern direkt den Wert (z. B. `int`, `bool`, `char`).
* **Referenztypen** speichern eine Referenz auf die Daten (z. B. `string`, `object`, Klassen).

### Primitive Datentypen

```csharp
int number = 5;              // Ganzzahl (32 Bit)
bool isActive = true;        // Wahrheitswert
char letter = 'A';           // Unicode-Zeichen
double average = 3.14;       // Gleitkommazahl (64 Bit)
decimal price = 19.99M;      // Dezimalwert mit hoher Genauigkeit
string name = "Anna";        // Zeichenkette (Referenztyp)
```

### var-Schlüsselwort

```csharp
var count = 10;     // compiler-inferierter Typ: int
var message = "Hi"; // compiler-inferierter Typ: string
```

> `var` muss immer mit Initialisierung verwendet werden und wird zur Compilezeit aufgelöst.

### Properties (Eigenschaften)

```csharp
public class Product {
    public string Name { get; set; }          // Auto-Property
    public decimal Price { get; private set; } // Nur lesbar von außen

    public bool InStock => Price > 0;          // Readonly-Property (Ausdrucksform)
}
```

* Properties kapseln Felder mit optionaler Logik (Getter/Setter).
* `get` liest den Wert, `set` weist einen neuen Wert zu.
* Auto-Properties (`{ get; set; }`) erstellen intern ein anonymes Feld.

### Nullable-Typen

```csharp
int? optional = null;
if (optional.HasValue) {
    Console.WriteLine(optional.Value);
}
```

* Mit `?` können Werttypen `null` zugewiesen bekommen.
* Alternative: Null-Koaleszenz (`??`), Null-Prüfung mit `?.` und `??=`.

### Konstanten und readonly

```csharp
const double Pi = 3.1415;        // zur Compile-Zeit festgelegt
readonly DateTime startTime = DateTime.Now; // zur Laufzeit initialisierbar
```

---

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

## 12. Entity Framework Core

Entity Framework Core (EF Core) ist ein modernes ORM (Object-Relational Mapper), das den Zugriff auf Datenbanken über C#-Objekte ermöglicht.

### Installation (per CLI)

```bash
# Für SQLite
$ dotnet add package Microsoft.EntityFrameworkCore.Sqlite

# Für SQL Server
$ dotnet add package Microsoft.EntityFrameworkCore.SqlServer

# Tools für Migrationen
$ dotnet add package Microsoft.EntityFrameworkCore.Tools
```

### Modell & DbContext

```csharp
public class User {
    public int Id { get; set; }
    public string Name { get; set; }
}

public class AppDbContext : DbContext {
    public DbSet<User> Users { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder options) =>
        options.UseSqlite("Data Source=app.db");
}
```

### Migration & Datenbank erzeugen

```bash
$ dotnet ef migrations add InitialCreate
$ dotnet ef database update
```

### CRUD-Operationen

```csharp
using var db = new AppDbContext();

// Create
var user = new User { Name = "Alice" };
db.Users.Add(user);
db.SaveChanges();

// Read
var users = db.Users.ToList();

// Update
user.Name = "Bob";
db.SaveChanges();

// Delete
db.Users.Remove(user);
db.SaveChanges();
```

### Hinweise:

* DbContext ist zentraler Einstiegspunkt zur Datenbank.
* Migrationen generieren Schemaänderungen in Code.
* Nutze `AsNoTracking()` für lesende Zugriffe ohne Change Tracking.
* Konfiguration kann auch über `appsettings.json` und DI erfolgen.

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
