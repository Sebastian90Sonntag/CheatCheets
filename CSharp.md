# C# Sprachreferenz – Cheat Sheet

## Inhaltsverzeichnis

* **Schlüsselwörter (Keywords)**
* **Datentypen (Types)**

  * Werttypen vs. Verweistypen
  * Nullable-Typen
  * Implizit typisierte Variablen (`var`) und dynamische Typen (`dynamic`)
* **Ausdrücke (Expressions)**
* **Operatoren (Operators)**
* **Pattern Matching (Musterabgleich)**
* **Anweisungen (Statements)**

  * Deklaration und Ausdrucks-Anweisungen
  * Auswahl/Verzweigung: `if`, `else`, `switch`
  * Schleifen: `for`, `foreach`, `while`, `do`
  * Sprunganweisungen: `break`, `continue`, `goto`, `return`, `yield`
  * Ausnahmebehandlung: `try`, `catch`, `finally`, `throw`
  * Sonstige: `checked/unchecked`, `lock`, `using`, `fixed`, `await`
* **Klassen und Strukturen (Classes & Structs)**
* **Schnittstellen (Interfaces)**
* **Aufzählungstypen (Enums)**
* **Records**
* **Delegates und Events**
* **Generics (Generische Typen)**
* **Attribute**
* **Namespaces und Using-Direktiven**
* **Präprozessor-Direktiven**
* **Unsicherer Code (Unsafe)**
* **XML-Dokumentation**

## Schlüsselwörter (Keywords)

C# besitzt eine Reihe vordefinierter, reservierter Schlüsselwörter mit spezieller Bedeutung im Code. Diese Wörter können nicht als Bezeichner (Variablennamen, etc.) benutzt werden, außer mit einem vorangestellten `@`-Zeichen (z.B. `@if` als Variablenname). Die folgende Tabelle listet alle *reservierten* Schlüsselwörter der Sprache C# (in alphabetischer Reihenfolge):

| abstract | as         | base    | bool     | break     | byte     | case      |
| -------- | ---------- | ------- | -------- | --------- | -------- | --------- |
| catch    | char       | checked | class    | const     | continue | decimal   |
| default  | delegate   | do      | double   | else      | enum     | event     |
| explicit | extern     | false   | finally  | fixed     | float    | for       |
| foreach  | goto       | if      | implicit | in        | int      | interface |
| internal | is         | lock    | long     | namespace | new      | null      |
| object   | operator   | out     | override | params    | private  | protected |
| public   | readonly   | ref     | return   | sbyte     | sealed   | short     |
| sizeof   | stackalloc | static  | string   | struct    | switch   | this      |
| throw    | true       | try     | typeof   | uint      | ulong    | unchecked |
| unsafe   | ushort     | using   | virtual  | void      | volatile | while     |

Zusätzlich gibt es *kontextabhängige* Schlüsselwörter, die nur in bestimmten Zusammenhängen eine besondere Bedeutung haben und sonst als Bezeichner verwendet werden dürfen. Beispiele für solche kontextabhängigen Keywords sind u.a. `var`, `dynamic`, `partial`, `async`, `await`, `record`, `with`, `nameof`, `and`, `or`, `not`, sowie die LINQ-Abfragebegriffe `from`, `where`, `select` usw. (Diese werden an entsprechender Stelle noch erläutert.) Neue Sprachfeatures werden nach Möglichkeit als kontextabhängige Keywords eingeführt, um Kompatibilitätsprobleme zu vermeiden.

## Datentypen (Types)

**Werttypen vs. Verweistypen:** C# unterscheidet zwischen *Value Types* (Werttypen) und *Reference Types* (Verweistypen). Eine Variable eines Werttyps enthält direkt den Wert (Daten) selbst, während eine Variable eines Verweistyps eine Referenz (einen Zeiger) auf ein Objekt im Heap enthält, das die eigentlichen Daten hält. Beispiel: Bei `int x = 5; int y = x;` erhält `y` eine Kopie des Wertes 5. Bei Klassenobjekten bewirkt `B = A;` hingegen, dass `B` auf dasselbe Objekt zeigt wie `A` – Änderungen über `B` wären dann auch über `A` sichtbar. Jede Zuweisung eines Werttyps erzeugt eine Kopie des gesamten Wertes. Werttypen können *nicht* `null` sein (außer man benutzt Nullable-Typen, s.u.), während Referenztypen `null` sein können (d.h. keine Referenz auf ein Objekt) und bei Zugriff darauf einen `NullReferenceException`-Fehler verursachen würden.

* **Werttypen:** Hierzu zählen alle einfachen eingebauten numerischen Typen (`int`, `double`, etc.), `bool` (Boolean), `char` (Zeichen), `decimal` (Dezimalzahl) sowie `enum` und `struct`-Typen. Sie leiten implizit von `System.ValueType` ab, welches wiederum von `System.Object` erbt. Alle Werttypen haben einen Standardwert (z.B. 0 für numerische Typen, `false` für bool, `'\0'` für char). Die *eingebauten* numerischen Werttypen in C# sind: **Ganzzahltypen** (8-bit: `byte`/`sbyte`; 16-bit: `short`/`ushort`; 32-bit: `int`/`uint`; 64-bit: `long`/`ulong`; sowie zeigerbreite Integer: `nint`/`nuint` seit C# 9) und **Gleitkomma** (`float` – 32-bit, `double` – 64-bit) sowie **`decimal`** (128-bit Dezimal). Zusätzlich gibt es den Typ **`bool`** (Wahrheitswert) und **`char`** (16-bit Unicode-Zeichen). Diese C#-Schlüsselwörter sind jeweils Aliase für .NET-Typen (z.B. `int` für `System.Int32`, `string` für `System.String` usw.) und können austauschbar verwendet werden.

* **Verweistypen:** Die wichtigsten Verweistypen sind **Klassen** (`class`-Typen), **Interfaces**, **Arrays**, **Delegates** und **`string`**. Klassen und Interfaces werden vom Programmierer definiert (siehe unten), Arrays sind Sammlungen (z.B. `int[]` ist ein Array von int-Werten) und `string` ist ein spezieller Referenztyp für Zeichenketten. Der Typ **`object`** (Alias für `System.Object`) ist der Basistyp, von dem *alle* anderen Typen erben. Man kann daher Werte *jedes* Typs einer `object`-Variablen zuweisen. (Ausnahme: *ref struct*-Typen, siehe weiter unten). Wird ein Werttyp als `object` behandelt, kommt *Boxing* zum Tragen: der Wert wird in ein Heap-Objekt verpackt. Umgekehrt wird beim Auslesen als Werttyp entboxed. Strings sind ebenfalls Referenztypen, werden aber in C# so behandelt, als hätten sie Wertsemantik – die Vergleichsoperatoren `==`/`!=` prüfen bei `string` den Inhaltsgleichheit statt die Referenzen. `string` ist *immutable*, d.h. unveränderlich – alle Methoden, die vermeintlich einen String ändern, erzeugen in Wirklichkeit einen neuen String.

**Built-in Type Aliases:** C# stellt für die wichtigsten .NET-Typen Aliase bereit. Die obigen Schlüsselwörter (`int`, `long`, `string`, etc.) – mit Ausnahme von `dynamic` und `delegate` – sind Aliasnamen für entsprechende .NET-Klassen. Beispielsweise sind `int` und `System.Int32` identisch. Der Typ `dynamic` verhält sich ähnlich wie `object` (intern werden alle `dynamic`-Ausdrücke auch als `object` behandelt) – mit dem Unterschied, dass bei `dynamic` alle Member-Aufrufe *zur Laufzeit* aufgelöst werden und der Compiler keine statische Typprüfung vornimmt. Dadurch kann man dynamische APIs (z.B. COM, IronPython) leichter nutzen, verliert aber die Typprüfung zur Compilezeit. Der *delegate*-Alias ist kein eigenständiger Laufzeittyp, sondern das Schlüsselwort zum Deklarieren neuer Delegatetypen (siehe **Delegates**). Schließlich gibt es den Typ **`void`**, der keine Werte darstellt und als Rückgabetyp für Methoden verwendet wird, die nichts zurückliefern.

### Werttypen und Nullable-Typen

Alle Werttypen (etwa int, float, bool, struct, etc.) können *normalerweise* keinen `null`-Wert annehmen. Oft besteht jedoch Bedarf, "kein Wert" für Werttypen zuzulassen – z.B. könnte eine Datenbank-Spalte vom Typ int auch `NULL` sein. Dafür gibt es **Nullable Value Types**: Durch Anhängen von `?` an den Typnamen wird ein Werttyp *nullable*. Beispielsweise ist `int?` (Alias für `Nullable<int>`) ein int-Wert, der zusätzlich auch `null` sein kann. Ein `bool?` kann die Werte `true`, `false` *oder* `null` enthalten. Technisch ist `T?` ein `System.Nullable<T>`-Strukturtyp. Auf einen Nullable-Werttyp kann man wie auf ein Struct mit zwei Eigenschaften zugreifen: `.HasValue` (gibt an, ob ein Wert vorhanden ist) und `.Value` (liefert den Wert, sofern nicht null). Meist prüft man aber einfach auf `null`, z.B. `if (age == null) {...}` oder man nutzt den *Pattern Match*: `if (age is int value) { /* value aus age extrahiert */ }`. Für `T?` gibt es eine automatische *Hebung* von Operatoren: Man kann z.B. zwei `int?` mit `<` vergleichen – das Ergebnis ist `false` oder `true` oder `null` (wenn einer der Operanden null ist).

Ab **C# 8** gibt es auch **Nullable Reference Types** (NRT), die durch ein angehängtes `?` bei *Referenztypen* angezeigt werden (z.B. `string?`). Damit kann man im Code explizit kennzeichnen, ob ein Referenztyp `null` sein darf oder nicht. Ist die Nullable-Referenztypen-Prüfung aktiviert (per Projekts-Einstellung oder `#nullable enable` Direktive), warnt der Compiler, wenn man möglicherweise `null` referenziert oder einer non-null-Variable irrtümlich `null` zuweist. Zum Beispiel: `string s = null;` würde einen Warnhinweis erzeugen, wenn `s` als non-null deklariert ist. Hingegen `string? s = null;` ist erlaubt, aber dann muss man vor Gebrauch von `s` eine `null`-Prüfung machen. Dieser Mechanismus hilft, Nullreferenz-Fehler zur Compilezeit zu erkennen. (Der Operator `!` kann eingesetzt werden, um dem Compiler mitzuteilen, dass eine bestimmte Expression garantiert nicht null ist – sogenannter *Null Forgiving Operator*, um Warnungen zu unterdrücken.)

### Implizit typisierte Variablen (`var`) und dynamische Typen (`dynamic`)

Das Schlüsselwort **`var`** kann verwendet werden, um lokale Variablen *implizit typisiert* zu deklarieren. Der tatsächliche Typ wird vom Compiler aus dem Initialisierungs-Ausdruck abgeleitet (Type Inference). Wichtig: `var`-Variablen sind dennoch statisch typisiert – der Typ wird einmalig beim Kompilieren festgelegt, als wäre er explizit angegeben. Beispiel: `var x = 5;` deklariert `x` als `int`, weil die rechte Seite ein Int32-Literal ist. Ohne Initialisierer kann `var` nicht verwendet werden. `var` erhöht die Lesbarkeit bei langen Typnamen und ist z.B. in LINQ-Abfragen nützlich, wo der genaue Typ komplex ist.

Der Typ **`dynamic`** dagegen bewirkt *dynamische* Typbindung. Eine Variable vom Typ `dynamic` umgeht die normale statische Typprüfung des Compilers – *jede* Methode oder Eigenschaft scheint für den Compiler zulässig, die tatsächliche Auflösung erfolgt erst zur Laufzeit. Dies ähnelt dem Spätbinden in Scriptsprachen. `dynamic` kann hilfreich sein bei COM-Interoperabilität (z.B. Office-Automation) oder bei JSON/XML-Daten, wo der Typ erst zur Laufzeit klar ist. Intern behandelt der Compiler dynamic-Operationen, indem er sie in Aufrufe der Dynamic Language Runtime (DLR) umwandelt. Zu beachten: `dynamic` ist quasi ein Alias für `object` zur Laufzeit – man kann *jeden* Wert einem `dynamic` zuweisen (implizite Konvertierung) und umgekehrt gibt es eine implizite Konvertierung von `dynamic` in jeden Typ (die dann aber zur Laufzeit geprüft wird). Bei Misuse kann es also zu Laufzeit-Ausnahmen kommen (RuntimeBinderException). Beispiel:

```csharp
dynamic d = "Hello"; 
int len = d.Length;      // ermittelt Länge des Strings "Hello" (5) zur Laufzeit
d.NonExistentMethod();   // **RuntimeBinderException**, falls zur Laufzeit keine solche Methode
```

Zusammenfassend: **`var`** – Typ wird vom Compiler einmalig inferiert (statisch), **`dynamic`** – Typprüfung erst zur Laufzeit.

## Ausdrücke (Expressions)

Ein **Ausdruck** in C# ist eine Konstruktion, die zu einem Wert führt oder einen Effekt hat. Die einfachsten Ausdrücke sind Literale (z.B. `42`, `"Hallo"` – feste Werte) und Bezeichner (Variablennamen). Durch **Operatoren** können diese zu komplexeren Ausdrücken kombiniert werden. Beispiel: In `c = a + b * 2` sind `a`, `b` und `2` einfache Ausdrücke, die mit `+` und `*` zu einem größeren Ausdruck verbunden sind. Operator-Präzedenz und -Assoziativität bestimmen, in welcher Reihenfolge die Teilausdrücke ausgewertet werden – im Beispiel wird `b * 2` vor dem `+` berechnet (weil `*` höher priorisiert ist); Klammern `()` können benutzt werden, um die Auswertungsreihenfolge explizit festzulegen.

Jeder Ausdruck hat einen Typ (z.B. `int`, `bool`, ein Klassen-Typ, usw.), außer in speziellen Fällen. Viele Ausdrücke *liefern einen Wert* und können als Teil eines größeren Ausdrucks eingebettet werden. Beispiel: `(x + 5) * Math.Max(y, 0)` – hier sind `x + 5` und `Math.Max(y,0)` Unterausdrücke, deren Ergebnisse weiterverwendet werden. Ein Methodenaufruf kann ebenfalls ein Ausdruck sein; liefert die Methode einen Rückgabewert, ist der Aufruf-Ausdruck vom entsprechenden Typ. Hat die Methode jedoch `void` als Rückgabetyp, dann erzeugt der Methodenaufruf *keinen* verwertbaren Wert – ein solcher `void`-Ausdruck darf nicht in einem größeren Ausdruck stehen, sondern kann nur als eigenständige Anweisung verwendet werden. Beispiel: `Console.WriteLine("Hi")` ist vom Typ void und kann nicht in `int x = Console.WriteLine(...);` erscheinen. Als alleinige Zeile `Console.WriteLine("Hi");` ist es aber gültig (Ausdrucks-Anweisung).

C# bietet verschiedene *spezielle Ausdrucksformen* zur komfortableren Syntax:

* **Interpolierte Strings:** Mit dem Präfix `$` vor einem string-Literal können im Text Platzhalter eingefügt werden, die Ausdrücke einbetten. Beispiel:

  ```csharp
  string name = "Alice";
  string msg = $"Hallo, {name.ToUpper()}!";
  // Ergebnis: "Hallo, ALICE!"
  ```

  Hier wird der Ausdruck `name.ToUpper()` in den String interpoliert. Interpolierte Strings ersparen mühsames Zusammenkonkatieren. (Ab C# 11 gibt es auch *Raw string literals* mit `"""` für mehrzeilige Strings und ohne Escape-Zeichen.)

* **Lambda-Ausdrücke:** Ein Lambda (`=>`-Syntax) definiert eine anonyme Funktion (Delegate) inline. Beispielsweise `x => x * x` ist ein Lambda, das ein Argument `x` entgegennimmt und `x * x` zurückgibt. Lambdas können mehrzeilig in `{ }`-Blöcken geschrieben werden und haben dann einen `return`. Sie werden häufig für Linq und als Callbacks verwendet.

* **LINQ-Query Expressions:** C# hat eine spezielle Query-Syntax ähnlich SQL. Beispiel:

  ```csharp
  int[] scores = { 90, 70, 100, 85 };
  var highScores = from s in scores
                   where s >= 80
                   orderby s descending
                   select s;
  ```

  Dieses Query-Expression wird in Methodenaufrufe übersetzt und liefert etwa die Elemente ≥80 in absteigender Reihenfolge. Die Query-Syntax ist optional; sie nutzt Keywords wie `from`, `where`, `select`, `orderby`, die kontextabhängig sind.

* **Ausdrucksform von Membern:** Methoden, Properties und Operatoren können als *expression-bodied members* definiert werden – eine verkürzte Syntax mit `=>`. Beispiel:

  ```csharp
  int Sum(int a, int b) => a + b;
  ```

  definiert eine Methode, die `a+b` zurückgibt, ohne Block. Ebenso kann man Properties schreiben: `public int X => _x;` anstelle eines get-Accessors. Diese Syntax erhöht die Prägnanz bei einfachen Definitionen.

* **Null-zu-Wert Konvertierung:** Der Null-Coalescing-Operator `??` und das *ternäre* `?:` (siehe **Operatoren**) sind ebenfalls Ausdrücke, die häufig für kompakte Zuweisungen genutzt werden.

C#-Ausdrücke liefern meist einen Wert und können wiederum Teil eines übergeordneten Ausdrucks sein. Beachte: Wenn ein Ausdruck keinen Wert ergibt (z.B. `void`-Methodenaufruf), kann er nicht direkt weiterverbunden werden, sondern nur in eine Statement konvertiert werden (siehe Ausdrucks-Anweisung).

## Operatoren (Operators)

C# stellt eine Vielzahl von Operatoren zur Verfügung, um Werte zu verknüpfen, zu vergleichen, zu modifizieren etc. Viele Operatoren funktionieren auf den eingebauten Datentypen (z.B. numerische Berechnungen), und viele können für eigene Typen *überladen* werden (mittels Operatorüberladung in Klassen/Structs). Hier ein Überblick wichtiger Operatoren und ihrer Funktion:

* **Arithmetische Operatoren:** `+` (Addition), `-` (Subtraktion), `*` (Multiplikation), `/` (Division) und `%` (Modulo, Rest). Diese gelten für numerische Typen. Zusätzlich `-x` (unäres Minus zur Vorzeichenumkehr) und `+x` (unäres Plus, hat keine Wirkung). Beispiel: `5 + 3 * 2` ergibt 11, da `*` Vorrang hat. *Inkrement/Dekrement:* `++` und `--` erhöhen bzw. verringern einen Wert um 1. Es gibt sie in Präfix- (`++x`) und Postfix-Form (`x++`), mit dem üblichen Unterschied: `++x` gibt den *erhöhten* Wert zurück, `x++` den ursprünglichen (und erhöht nach Auswertung).

* **Vergleichs- und Gleichheitsoperatoren:** `==` prüft auf Gleichheit, `!=` auf Ungleichheit. Für sortierbare Typen (Zahlen, `char`, `DateTime` etc.) gibt es `<`, `>` (kleiner, größer) sowie `<=`, `>=` (kleiner-gleich, größer-gleich). Diese Operatoren liefern einen booleschen Wert (`true`/`false`). Bei Referenztypen testet `==` standardmäßig Referenzgleichheit, außer bei `string` (dort inhaltlich überladen) oder wenn in einer Klasse überladen.

* **Logische Operatoren (boolesche Logik):** `&&` (logisches UND mit Kurzschlussauswertung), `||` (logisches ODER mit Kurzschluss) und `!` (NOT) werden auf booleschen Werten verwendet. `A && B` ergibt `true` genau dann, wenn *beide* Operanden `true` sind (und wertet B **nicht** aus, falls A schon `false` ist). `A || B` ist `true` wenn *mindestens einer* `true` ist (B wird nicht mehr ausgewertet, falls A `true` ist). `!x` negiert den Wahrheitswert. Es gibt auch bitweise/logische Operatoren `&` und `|` sowie `^` (XOR), die auf bool *ohne* Kurzschluss wirken **und** auf Integer-Bits (siehe unten). Meist nutzt man für bool jedoch `&&`/`||` statt `&`/`|` wegen Kurzschluss.

* **Bitoperatoren:** Für ganzzahlige Typen führen `&` (Bit-AND), `|` (Bit-OR) und `^` (Bit-XOR) bitweise Operationen durch. `~x` ist der bitweise NOT (Komplement aller Bits, 1->0 und 0->1). Schiebeoperatoren `<<` (Bits nach links schieben) und `>>` (Bits nach rechts schieben) gibt es für int, uint, long, ulong. In C# 11 wurde zusätzlich `>>>` eingeführt als *logischer* Shift-Rechts für *signed* Zahlen (führt mit Nullen, um das Vorzeichenbit nicht immer mitzuschieben). Beispiel: `5 << 1` ergibt 10 (binär 0101 -> 1010).

* **Zuweisungen:** Der Zuweisungsoperator ist `=`. Er weist den rechten Wert der linken Variablen zu. Zusätzlich gibt es kombinierte Zuweisungen, z.B. `+=`, `-=`, `*=`, `/=`, `%=` etc., die eine Operation mit anschließender Zuweisung durchführen. Beispiel: `x += 5;` entspricht `x = x + 5;`. Ebenso `&=`, `|=`, `^=`, `<<=` usw. für Bitoperationen. Speziell: `??=` ist der Null-Koaleszierende Zuweisungsoperator – `x ??= y;` weist `y` nur dann `x` zu, *wenn* `x` aktuell `null` ist.

* **Null-Coalescing (`??`):** Der Operator `??` ist ein binärer Operator, der `A ?? B` wie folgt auswertet: Er liefert `A` zurück, falls `A` nicht null ist; andernfalls den Wert von `B`. Damit lassen sich Null-Werte elegant abfangen. Beispiel: `string name = inputName ?? "Default";` – wenn `inputName` null war, wird `"Default"` genommen. Kombiniert mit Zuweisung (oben `??=`) kann man auch in-place eine Null-Variable setzen.

* **Bedingungsoperator (`?:`):** Dies ist der *ternäre* Operator (drei Operanden). Syntax: `condition ? exprTrue : exprFalse`. Wenn `condition` (bool) true ist, wird `exprTrue` ausgewertet und liefert das Resultat, sonst `exprFalse`. Beispiel: `int max = (a > b) ? a : b;` – `max` bekommt den größeren der beiden. Dies entspricht einer verkürzten if/else-Auswahl als Ausdruck. Beide Zweige müssen typkompatibel sein (gleicher Ergebnistyp).

* **Typprüfung und -konvertierung:** Der Operator **`is`** prüft, ob ein Objekt einen bestimmten Typ oder ein bestimmtes Muster hat. Z.B. `if (obj is string)` prüft, ob `obj` ein String ist. In neueren C#-Versionen kann `is` auch *Pattern Matching* (siehe Abschnitt *Pattern Matching*) durchführen, z.B. `if (obj is Person p && p.Age > 18)`. Der **`as`**-Operator versucht, eine Referenz in einen gegebenen Typ zu casten und gibt entweder ein Objekt des Zieltyps zurück oder `null` bei Misserfolg (anstatt eine Exception zu werfen). Beispiel: `Person p = obj as Person;` gibt `p` oder null. Ein normaler Typ-Cast `(T)expr` konvertiert zwischen kompatiblen Typen oder wirft eine InvalidCastException, falls unzulässig. **`typeof(T)`** gibt ein `System.Type`-Objekt für den Typ `T` zurück (zur Reflexion). **`nameof(x)`** liefert den Namen eines Symbols `x` als String (z.B. bei `nameof(Person.Age)` -> `"Age"`, verwendet um refactoring-sichere Strings für Log-Ausgaben etc. zu erhalten). **`sizeof(T)`** ergibt die Größe (in Bytes) eines unmanaged-Typs `T`. `sizeof` kann z.B. für primitive Typen verwendet werden (`sizeof(int)` ist 4); für benutzerdefinierte Structs nur in unsafe-Kontext.

* **Objekt-Erzeugung:** Der **`new`**-Operator erstellt ein neues Objekt oder Array. Z.B. `new MyClass()` ruft den Konstruktor von MyClass auf und liefert eine Referenz auf das neue Objekt. `new int[10]` erzeugt ein int-Array der Länge 10. (Ab C# 9 kann man bei einer Variablen mit bekanntem Typ auf der rechten Seite den Typ weglassen: `List<int> list = new();` – *Target-typed new*.)

* **Nullbedingte Operatoren:** Der **Null-conditional Operator** `?.` erlaubt das sichere Navigieren durch Referenztypen. Beispiel: `obj?.Property` liefert `null` statt einer Exception, falls `obj` selbst `null` ist. Ebenso `obj?.Method()` ruft die Methode nur auf, wenn `obj` nicht null ist (sonst Ergebnis null). Es gibt auch `?[]` für indizierten Zugriff auf Arrays/Listen mit Nullprüfung. Diese Operatoren verkürzen Kaskaden von null-Abfragen. Sie lassen sich mit `??` kombinieren, um Defaultwerte bei Null bereitzustellen.

**Operatorrang und Assoziativität:** In mehrteiligen Ausdrücken gilt eine feste Präzedenzordnung. Z.B. wird `*` vor `+` ausgewertet, `==` vor `&&` usw. Die genaue Rangfolge ist in der Sprache definiert. Man kann immer Klammern nutzen, um die Auswertungsreihenfolge explizit festzulegen. *Assoziativität* bestimmt, wie bei gleicher Priorität von links oder rechts gruppiert wird. Die meisten binären Operatoren sind *linksassoziativ* (werden von links nach rechts ausgewertet) – z.B. `a - b - c` wird als `(a - b) - c` geparst. Einige sind *rechtsassoziativ*, insbesondere der Zuweisungsoperator `=` und der ternäre `?:` – d.h. `x = y = 5` wird als `x = (y = 5)` ausgewertet. In Zweifelsfällen kann Klammern setzen Klarheit schaffen.

## Pattern Matching (Musterabgleich)

Moderne C#-Versionen (ab C# 7.0 und umfangreicher ab C# 8+) unterstützen **Pattern Matching**, d.h. das Prüfen und Zerlegen von Werten nach bestimmten *Mustern*. Pattern Matching tritt im Zusammenhang mit dem `is`-Operator, in `switch`-Anweisungen und in *Switch Expressions* auf. Man kann ein gegebenes Objekt/Value also gegen verschiedene Muster testen und je nach Treffer unterschiedliche Aktionen ausführen.

Unterstützte **Pattern-Arten** sind u.a.:

* **Type Pattern / Declaration Pattern:** Prüft den Laufzeittyp und legt bei Erfolg eine Variable an. Syntax: `expr is T name`. Beispiel: `if (obj is Person p)` – wenn `obj` zur Laufzeit vom Typ `Person` (oder abgeleitet) ist, wird `p` als Person deklariert und innerhalb des if-Bereichs verwendet. Dieses *Deklarationspattern* kombiniert Typprüfung mit Casting. (Ohne Muster hätte man `if(obj is Person) { Person p = (Person)obj; ... }` schreiben müssen.)

* **Constant Pattern:** Prüft auf einen konstanten Wert. Beispiel: `case 0:` innerhalb eines `switch` – entspricht `x == 0`. Konstanten Patterns können Literale, Enum-Konstanten oder `null` sein, etc. Z.B. `if (s is null)` prüft auf null.

* **Relational Patterns:** Vergleichen einen Wert mit einem Konstanten mittels `<`, `<=`, `>`, `>=`. Beispiel: `x is > 0` ergibt true, wenn `x` größer 0 ist. In `switch`-Ausdrücken kann man so Bereiche prüfen: `case < 0:`.

* **Logical Patterns:** Kombinieren Patterns mittels `and`, `or`, `not` (Schlüsselwörter für logische Verknüpfung in Patterns). Beispiel: `if (x is >= 0 and <= 100)` – wahr, wenn x zwischen 0 und 100 liegt. Oder `case <= 0 or >= 100:` im Switch – trifft auf Werte ≤0 oder ≥100 zu. `not P` negiert ein Pattern P.

* **Property Pattern:** Prüft innere Eigenschaften eines Objekts auf Muster. Syntax z.B. `obj is Person { Age: > 18, Name: { Length: > 0 } }`. Hier wird getestet, ob `obj` eine Person mit `Age > 18` und `Name.Length > 0` ist. Im Switch kann man z.B. `case Person { Age: < 18 }: ...` verwenden.

* **Positional Pattern:** Zerlegt ein Objekt über Deconstruction (oder Tuple) in Komponenten und prüft diese. Beispiel bei einem Tupel: `case (int x, int y) when x == y:` – zwei Werte gleich. Oder bei einem `record Point(int X, int Y)`: `case Point(0, 0):` für den Ursprung. Für Listen/Array gibt es seit C# 11 **List Patterns**, um Elemente zu matchen, z.B. `case [var first, _, var last]:` passt auf eine Liste mit mindestens 2 Elementen und bindet das erste und letzte Element.

* **Var Pattern:** `var x` im Pattern matcht *alles* und bindet es an x. Dies ist nützlich, um z.B. in einem `switch` einen "Catch-All" Fall mit Zugriff auf den Wert zu haben. Vergleichbar mit einem `default`, aber mit Benennung. Ebenfalls gibt es den **Discard Pattern** `_`, ein Platzhalter, der alles matcht und nichts bindet (wie ein anonymes var). Wird oft genutzt, um bestimmte Teile zu ignorieren, z.B. `case (0, _)` in einem Tupel-switch – erster Wert 0, zweiter egal.

**Pattern Matching in `switch`:** Die klassische `switch`-Anweisung erlaubt seit C# 7 Patterns in den `case`-Labels anstelle von konstanten Werten. Beispiel:

```csharp
object obj = ...;
switch(obj)
{
    case int n when n > 0:
        Console.WriteLine($"Positive Zahl: {n}");
        break;
    case string s:
        Console.WriteLine($"String der Länge {s.Length}");
        break;
    case null:
        Console.WriteLine("obj ist null");
        break;
    default:
        Console.WriteLine("Anderer Typ oder Fall");
        break;
}
```

Hier zeigt `case int n when n > 0:` ein **Type Pattern** (mit **when**-Klausel als zusätzlicher Bedingung). Der Wert `obj` wird geprüft, ob er ein `int` ist und >0, dann nach `n` konvertiert. Patterns können also mit einer `when`-Klausel weiter eingeschränkt werden (z.B. zusätzliche Bedingungen, die nicht allein durch Pattern ausdrückbar sind). Der `default`-Zweig fängt alles ab, was kein vorheriges Pattern erfüllt.

**Switch Expressions:** Seit C# 8 gibt es eine neue Ausdrucksform: `var result = expr switch { pattern1 => value1, pattern2 => value2, _ => defaultValue };`. Ein Switch-Expression liefert einen Wert und ersetzt oft lange ternäre Ausdrücke. Beispiel:

```csharp
string DetermineSign(int x) => x switch
{
    > 0    => "positive",
    0      => "zero",
    < 0    => "negative"
};
```

Hier werden relationale Patterns `> 0`, `0`, `< 0` genutzt. Das `_`-Pattern (Discard) fängt alle übrigen Fälle (hier eigentlich keiner übrig) ab. Switch Expressions müssen *erschöpfend* sein (alle möglichen Werte müssen abgedeckt sein, sonst Compilerwarnung). Sie sind expressions, d.h. ohne explizites `break` und mit Komma getrennt.

**Nondestructive Mutation via `with`:** Für `record`-Typen gibt es den speziellen **`with`-Ausdruck**: `var p2 = p1 with { PropertyX = neueWert };`. Dieser kopiert das Objekt `p1` in ein neues Objekt `p2`, wobei die angegebene Property geändert wird (alle anderen bleiben gleich). Dies ermöglicht es, Immutable-Objekte bequem "geändert" zu bekommen, ohne das Original zu verändern (d.h. *ohne* destructive update). Der `with`-Syntax kann nur auf `record`-Objekte angewendet werden und nutzt im Hintergrund den von Records generierten *Copy Constructor* sowie die init-only Properties.

Zusammengefasst macht Pattern Matching den Code ausdrucksstärker und vermeidet viele manuelle Casts oder Kaskaden von `if-else`-Abfragen. Man kann damit in einer eleganten Weise nach Typen verzweigen, Inhalte von Objekten prüfen und Werte analysieren, was besonders in Kombination mit Records und TUples hilfreich ist.

## Anweisungen (Statements)

Ein C#-*Statement* ist die kleinste ausführbare Einheit eines Programms, quasi ein Befehl. Statements werden in der Regel durch `;` abgeschlossen oder sind Blockanweisungen in `{ }`. Mehrere Statements können in einem `{ ... }`-Block zusammengefasst werden. Hier die wichtigsten Anweisungsarten in C#:

* **Deklarations-Anweisung:** deklariert eine Variable oder Konstante. Beispiel: `int x;` oder `const double PI = 3.14;`. Eine Variable kann optional direkt einen Wert erhalten (Initialisierung), bei `const` ist die Initialisierung Pflicht (da Konstanten sofort festgelegt sein müssen). Deklarationsstatements erzeugen keine sichtbare Ausgabe, sondern reservieren Speicher oder definieren Konstanten.

* **Ausdrucks-Anweisung:** Ein Ausdruck, der in ein Statement konvertiert wurde, typischerweise um *Nebenwirkungen* zu erzielen. Viele gängige Statements sind eigentlich Ausdrucks-Statements: Zuweisungen (`x = 5;`), Methodenaurufe (`DoSomething();`), ++/-- Operationen allein auf einer Zeile (`i++;`), oder `new`-Objekterstellungen ohne weitere Verwendung (`new MyClass();`). Wichtig: Ein reiner Ausdruck, der nur einen Wert berechnet aber nicht verwendet, ist als Statement *nicht* erlaubt (weil es vermutlich ein Programmierfehler wäre). Z.B. `x + 1;` allein ergibt keinen Sinn und führt zu einem Compilefehler ("Expression ist kein Statement"). Man muss das Ergebnis zumindest zuweisen oder nutzen. Daher: `x = x + 1;` ist gültig (und das übliche Muster, statt `x++`). Der Compiler erzwingt damit, dass keine Berechnungen ins Leere laufen.

* **Auswahl-/Verzweigungsanweisungen:** ermöglichen bedingte Ausführung von Code.

  * **`if`/`else`:** Führt je nach boolescher Bedingung einen Block aus. Syntax: `if (Bedingung) { ... } else { ... }`. Der `else`-Teil ist optional. Beispiel:

    ```csharp
    if (score >= 50) {
        Console.WriteLine("Bestanden");
    } else {
        Console.WriteLine("Durchgefallen");
    }
    ```

    Hier wird je nach Wahrheitswert von `score >= 50` einer der beiden Blöcke ausgeführt. Es können auch mehrere if/else-Ketten gebildet werden, etwa `if (...) {...} else if (...) {...} else {...}`.

  * **`switch`-Anweisung:** ermöglicht Mehrfachverzweigungen auf Basis eines Ausdrucks. Eine klassische `switch` vergleicht einen Wert (oft `int`, `string`, `enum` etc.) gegen verschiedene `case`-Konstanten oder Muster (siehe Pattern Matching oben). Jeder `case`-Abschnitt endet typischerweise mit `break;` (oder einem Sprung wie `return`) um das Durchfallen (*fall-through*) zu verhindern. Beispiel:

    ```csharp
    switch(dayOfWeek) {
      case 0:
      case 6:
        Console.WriteLine("Wochenende");
        break;
      case 1:
      case 2:
      case 3:
      case 4:
      case 5:
        Console.WriteLine("Wochentag");
        break;
      default:
        Console.WriteLine("Ungültiger Wochentag");
        break;
    }
    ```

    Hier werden 0 und 6 (Samstag, Sonntag) zusammengefasst behandelt. Das `default`-Label fängt alle nicht explizit behandelten Werte ab. Ab C# 8+ können `case`-Labels auch Muster enthalten (z.B. `case >= 100:`), wodurch `switch` mächtiger wird.

    > *Hinweis:* In C# muss jeder `case`-Block getrennt enden (typischerweise mit `break`). "Durchfallende" Cases ohne `break` sind nicht erlaubt (außer man schreibt den Case-Inhalt absichtlich leer und bündelt wie oben). Dies verhindert viele Fehler, die in Sprachen wie C/C++ durch fehlende Breaks entstehen könnten.

  * **`switch` Expression:** bereits oben im Pattern Matching erläutert, ist ein Ausdruck, keine Statement. Für Statement-Zwecke kann man stattdessen oft Ketten von if/else oder das klassische switch Statement verwenden.

* **Iterationen (Schleifen):** führen einen Codeblock wiederholt aus.

  * **`for`-Schleife:** klassische Zählschleife mit Initialisierung, Abbruchbedingung und Iterationsausdruck. Syntax: `for (Initialisierung; Bedingung; Iteration) { ... }`. Beispiel:

    ```csharp
    for(int i = 0; i < 5; i++) {
        Console.WriteLine(i);
    }
    ```

    Hier wird `i` von 0 bis 4 durchlaufen. Die drei Steuer-Teile sind alle optional; man kann z.B. `for(;;)` schreiben (endlose Schleife). Typischerweise dient `for` zum Durchlaufen von Indexbereichen oder ähnlichem.

  * **`foreach`-Schleife:** vereinfacht das Durchlaufen aller Elemente einer Auflistung (Array, `IEnumerable`, etc.). Syntax: `foreach (ElementTyp name in Sammlung) { ... }`. Beispiel:

    ```csharp
    string[] words = { "foo", "bar", "baz" };
    foreach(string w in words) {
        Console.WriteLine(w);
    }
    ```

    Hier wird automatisch von Anfang bis Ende iteriert. Im Gegensatz zur `for` muss man sich nicht um Indexgrenzen kümmern. **Wichtig:** Standardmäßig iteriert `foreach` *by value* – d.h. bei Werttypen arbeitet man auf Kopien, Änderungen an `w` im Beispiel oben ändern nicht das Array. (Ab C# 7.3 kann man mit `foreach(ref T item in collection)` auch by-ref iterieren.)

  * **`while`-Schleife:** führt einen Block aus, solange eine Bedingung true ist (Kopf-gesteuerte Schleife). Beispiel:

    ```csharp
    while(count > 0) {
        DoWork();
        count--;
    }
    ```

    Hier wird vor jedem Durchlauf `count > 0` geprüft; sobald diese Bedingung false wird, endet die Schleife. Wenn die Bedingung von Anfang an false ist, wird der Rumpf kein einziges Mal ausgeführt.

  * **`do { ... } while(...);`:** ähnlich wie while, aber *fußgesteuert* – d.h. der Block wird *mindestens einmal* ausgeführt, weil die Bedingung erst **nach** dem ersten Durchlauf geprüft wird. Beispiel:

    ```csharp
    string input;
    do {
        input = Console.ReadLine();
    } while(String.IsNullOrEmpty(input));
    ```

    Hier wird der Benutzer so lange zur Eingabe aufgefordert, bis `input` nicht leer ist.

  Alle Schleifen können durch die unten genannten Sprunganweisungen beeinflusst werden (`break`, `continue`).

* **Sprunganweisungen:** steuern den Ablauf, indem sie aus Schleifen/Blöcken springen oder diese beeinflussen.

  * **`break`:** bricht die *umgebende* Schleife oder `switch`-Anweisung sofort ab. Bei verschachtelten Schleifen springt es nur aus der innersten heraus. In einem `switch` beendet es den aktuellen case-Block. Beispiel: in einer `for`-Schleife kann `if(someCondition) break;` verwendet werden, um vorzeitig auszusteigen.

  * **`continue`:** bricht den aktuellen Durchlauf einer Schleife ab und springt zum nächsten Iterationszyklus (d.h. zurück an den Schleifenkopf). Beispiel: in einer `for` kann `if(skip) continue;` genutzt werden, um die restlichen Anweisungen der Schleife zu überspringen und sofort mit dem nächsten `i` weiterzumachen.

  * **`goto`:** ein direkter Sprung zu einem benannten Label im selben Block. Syntax: `labelname: ...` zum Label definieren, und `goto labelname;` um hinzuspringen. Goto sollte in Hochsprachen fast nie verwendet werden, außer in speziellen Fällen (z.B. aus tief verschachtelten Loops mehrfach breaken, oder im Switch um "Fallthrough" zu erreichen). Besser lesbare Alternativen sind meist vorhanden. Es gibt auch `goto case X;` und `goto default;` innerhalb eines switch, um zu einem anderen Case-Label zu springen (selten genutzt).

  * **`return`:** verlässt die aktuelle Methode und gibt ggf. einen Wert zurück. In einer `void`-Methode kann man `return;` ohne Wert schreiben. In einem Wert zurückgebenden Methodentyp muss ein Ausdruck folgen: `return x+y;`. `return` kann auch aus einem Methodenrumpf innerhalb z.B. eines `if` kommen, womit man eine Methode vorzeitig beenden kann.

  * **`yield return` / `yield break`:** Diese Keywords kommen in *Iterator*-Methoden zum Einsatz (Methoden, die einen `IEnumerable`/`IEnumerator` zurückgeben). `yield return value;` gibt einen Wert an den Aufrufer zurück, pausiert die Methode aber an dieser Stelle – der nächste Aufruf von `MoveNext()` (bei `foreach` implizit) setzt nach dieser Stelle fort. Die Position im Code wird also gemerkt. `yield break;` kann benutzt werden, um die Iteration vorzeitig zu beenden (entspricht einem `return` in normalen Methoden, aber nur den Enumerator schließen). Iterator-Methoden erlauben so ein bequemes Generieren von Sequenzen. Beispiel:

    ```csharp
    IEnumerable<int> CountDown(int start) {
        for(int i=start; i>=0; i--) {
            yield return i;
        }
        // yield break; optional am Ende, implizit
    }
    ```

    Bei jedem `yield return` wird ein Wert produziert und im Aufrufer als nächstes Element geliefert, während die Methode eingefroren wird, bis das nächste Element angefordert wird.

* **Ausnahmebehandlung (Exception Handling):** dient zum Abfangen und Behandeln von Fehlerzuständen zur Laufzeit.

  * **`throw`**: Mit `throw` wird eine Exception ausgelöst (*geworfen*). Entweder erstellt man eine neue Exception: `throw new InvalidOperationException("Fehler XY");` oder man rethrowt in einem Catch eine gefangene Exception mit einfachem `throw;`. Letzteres erhält den originalen Stack-Trace, während `throw ex;` den Stacktrace neu setzt (daher sollte man in C# nach einem Catch meist `throw;` ohne Objekt verwenden, um die Ausnahme weiterzureichen).

  * **`try` / `catch` / `finally`:** Ein `try`-Block umschließt Code, der Exceptions werfen könnte, und ermöglicht durch angehängte `catch`-Blöcke die Behandlung bestimmter Ausnahme-Typen. Ein optionaler `finally`-Block wird *immer* ausgeführt, egal ob eine Exception auftrat oder nicht – typisch für Aufräumarbeiten (z.B. Datei schließen). Beispiel:

    ```csharp
    try {
        int result = DangerousOperation();
        Console.WriteLine($"Result: {result}");
    } catch(FormatException ex) {
        Console.WriteLine("Format-Fehler: " + ex.Message);
    } catch(Exception e) when(e.Message.Contains("XYZ")) {
        Console.WriteLine("Gefilterter Fehler: " + e.Message);
    } finally {
        Console.WriteLine("Aufräumen");
    }
    ```

    Hier werden FormatException speziell und alle anderen Exception mit einem *Filter* (`when`-Klausel) auf Message "XYZ" abgefangen. Alle nicht gefangenen Exceptions propagieren weiter zum Aufrufer. Im `finally`-Block würde man z.B. Ressourcen freigeben. Falls in keinem `catch` abgefangen wird, gelangt die Exception ggf. höher (bis ggf. zum Programmende, wo sie das Programm abstürzen lässt, falls nicht abgefangen). **Exception-Filter** (`catch(... when(condition))`) erlauben es, eine Ausnahme nur bei bestimmten Bedingungen zu fangen – ist die Bedingung false, wird der Catch übersprungen und die Exception so behandelt, als wäre der Catch nicht da (d.h. ggf. an nächste Catch höher gegeben).

    Mehrere `catch`-Blöcke können gestapelt werden. Sie werden der Reihe nach geprüft, bis einer passt. Specifiche Exceptions sollte man vor allgemeineren fangen (also z.B. zuerst `catch(FileNotFoundException)`, dann `catch(IOException)`, dann `catch(Exception)` ganz zuletzt). Ab C# 7 darf ein `catch` auch ein Pattern nutzen: z.B. `catch (IOException ex) when (ex is FileNotFoundException)` – allerdings kann man genauso gut einen separaten FileNotFound catch schreiben.

  * **`using`-Anweisung:** (`using (...) { ... }`) ist eine spezielle Sprachkonstruktion, um Ressourcen sicher freizugeben. Sie erwartet, dass innerhalb der Klammer ein Objekt erzeugt wird, das `IDisposable` implementiert. Nach Verlassen des Blocks ruft `using` automatisch dessen `.Dispose()`-Methode auf, auch wenn innerhalb eine Exception fliegt. Damit ist sichergestellt, dass z.B. Dateien, Datenbankverbindungen, Streams etc. geschlossen werden. Beispiel:

    ```csharp
    using(var file = File.OpenText("data.txt")) {
        string content = file.ReadToEnd();
        // file.Dispose() wird *automatisch* im Hintergrund im Finally aufgerufen
    }
    ```

    Das obige entspräche im Prinzip:

    ```csharp
    var file = File.OpenText("data.txt");
    try {
       ... 
    } finally {
       if(file != null) file.Dispose();
    }
    ```

    Seit **C# 8.0** gibt es zusätzlich *Using Declarations*: Man kann `using var file = ...;` schreiben **ohne** umschließenden Block. Die Variable `file` gilt dann bis zum Ende des aktuellen Blocks und ihr Dispose wird implizit im `finally` beim Blockende aufgerufen. Das spart Einrückung. Allerdings sollte man auf die Gültigkeit achten – oft ist ein normaler using-Block klarer.

* **Andere Anweisungen:**

  * **`checked` / `unchecked`:** Diese ermöglichen die Kontrolle, wie arithmetische Überläufe bei ganzzahligen Rechnungen gehandhabt werden. In einem `checked`-Kontext (Block oder Operator) werfen z.B. Überläufe eine `OverflowException`. Standardmäßig (Compiler-Default im Release-Build) sind arithmetische Überläufe *unchecked* (d.h. Bits wrap around ohne Exception). Beispiel:

    ```csharp
    int a = int.MaxValue;
    int b = unchecked(a + 1); // Überlauf, aber kein Fehler (b wird int.MinValue)
    int c = checked(a + 1);   // OverflowException zur Laufzeit
    ```

    Man kann auch das gesamte Projekt auf checked stellen (/checked Flag), dann wären nur gezielte `unchecked`-Blöcke Ausnahmen.

  * **`lock`-Anweisung:** Syntax: `lock(obj) { ... }` dient der Thread-Synchronisation. Es sorgt dafür, dass zu jedem Zeitpunkt nur *ein* Thread den geschützten Codeblock ausführen kann, indem es einen exklusiven Monitor-Lock auf `obj` nimmt. Andere Threads, die auch `lock(obj)` verwenden, müssen warten bis der Lock frei wird. Damit kann man shared Ressourcen threadsicher machen. Üblich wird ein privates Objekt als Lock verwendet: z.B. `private readonly object _lock = new object(); ... lock(_lock) { ... }`. Dies entspricht Monitor.Enter/Exit-Aufrufen im .NET-Framework, aber `lock` stellt auch sicher, dass der Lock im Falle einer Exception wieder freigegeben wird (da es im Hintergrund in einen try/finally gepackt ist).

  * **`fixed`-Anweisung:** kommt im Unsafe-Code zum Einsatz. `fixed(Type* ptr = expr) { ... }` *pinnt* ein Objekt im Speicher während des Blocks, damit es der Garbage Collector nicht verschiebt. Innerhalb des Blocks kann man dann einen Zeiger (`ptr`) verwenden, der garantiert gültig ist. Z.B.:

    ```csharp
    fixed(byte* p = bytesArray) {
       // p bleibt gültig auch wenn GC läuft
       byte first = *p;
    }
    ```

    Ohne `fixed` dürften Zeiger auf Managed-Objekte nicht verwendet werden, da GC sie verschieben könnte. Nach Verlassen des Blocks wird das Objekt wieder freigegeben für Verschiebungen.

  * **`await`-Anweisung/Operator:** In einer Methode, die mit dem Modifier `async` gekennzeichnet ist, kann der **`await`**-Operator verwendet werden, um asynchron auf ein Ergebnis zu warten. `await` erwartet ein `Task` oder `Task<T>` (bzw. ein kompatibles awaitable) und pausiert die Ausführung der Methode, bis der Task abgeschlossen ist. Währenddessen kehrt die Kontrolle an den Aufrufer zurück (die async-Methode gibt ein Task zurück und setzt ihren Ablauf später fort). Beispiel:

    ```csharp
    public async Task<int> GetDataAsync() {
       HttpClient client = new HttpClient();
       string data = await client.GetStringAsync(url);
       Console.WriteLine("Download fertig");
       return data.Length;
    }
    ```

    Hier pausiert die Methode beim `await client.GetStringAsync(...)` und das Warten geschieht ohne den Thread zu blockieren – bei Resume wird weitergemacht. Async/Await ermöglicht nebenläufige, nicht-blockierende Abläufe auf einfache Weise. Technisch wird bei `await` der restliche Methodencode als Callback registriert und die Methode gibt zwischenzeitlich zurück. **Wichtig:** `await` darf *nur* innerhalb von `async`-Methoden verwendet werden. Das Schlüsselwort `async` am Methodenkopf signalisiert dem Compiler, eine state machine für await zu bauen. In Main/oberster Kontext von C# kann man ab C# 7.1 auch `async Task Main` nutzen.

## Klassen und Strukturen (Classes & Structs)

**Klassen** und **Structs** sind die grundlegenden benutzerdefinierten Typen in C#. Beide können Felder, Methoden und andere Member enthalten, unterscheiden sich aber in ihrem Semantik (Referenztyp vs Werttyp).

* Eine **Klasse** (`class`) definiert einen *Referenztyp*. Instanzen werden mit `new` erstellt und auf dem Managed Heap gespeichert, Variablen vom Klassentyp sind Referenzen darauf. Klassen unterstützen *Vererbung*: Sie können von einer Basisklasse erben (einfaches Vererbung, keine Mehrfachvererbung von Klassen) und so deren Mitglieder übernehmen. Methoden in Klassen können als `virtual` deklariert werden, um in abgeleiteten Klassen via `override` überschrieben zu werden (Polymorphie). Klassen können auch als `abstract` markiert sein – dann können keine Objekte direkt davon erstellt werden, und sie dürfen abstrakte Methoden (ohne Implementierung) enthalten, die in Unterklassen implementiert werden müssen. Umgekehrt können Klassen als `sealed` deklariert werden, um weitere Vererbung zu verhindern. Standardmäßig erbt jede Klasse (die nichts anderes angibt) von `System.Object`. Beispiel einer Klassendeklaration:

  ```csharp
  public class Person : Human, IComparable<Person> 
  {
      // Felder:
      private string name;
      public const int SpeciesCount = 1;
      
      // Eigenschaft:
      public string Name {
          get => name;
          set => name = value;
      }
      
      // Konstruktor:
      public Person(string name) {
          this.name = name;
      }
      
      // Methode:
      public void SayHello() {
          Console.WriteLine($"Hi, I'm {name}");
      }
      
      // Statische Methode:
      public static Person CreateUnnamed() => new Person("Unknown");

      // Override einer vererbten virtuellen Methode:
      public override string ToString() => $"Person({name})";

      // Interface-Implementierung:
      public int CompareTo(Person other) => string.Compare(this.name, other.name);
  }
  ```

  Hier erbt `Person` von `Human` (einer anderen Klasse) und implementiert das generische Interface `IComparable<Person>`. Sie hat ein privates Feld, eine öffentliche Konstante, eine Auto-Property `Name` mit internem Feld, einen Konstruktor, eine Instanzmethode, eine statische Fabrikmethode und overrideet die `ToString()`-Methode von `object`.

  * **Mitglieder von Klassen:**

    * *Felder* (Instanzfelder) halten Daten pro Objekt. Sie können `public`/`private`/... sein. Mit `static` deklarierte Felder sind Klassenfelder (eine gemeinsame Speicherstelle für die ganze Klasse, unabhängig von Instanzen).
    * *Methoden* definieren Verhalten. Mit `static` sind sie Klassenmethoden (ohne Zugriff auf Instanzzustand, aufzurufen über `Klasse.Methodenname()`).
    * *Eigenschaften* (Properties) sind Syntax-Sugar für Getter/Setter-Methoden, um Felder kontrolliert zu lesen/schreiben. Sie werden wie Felder benutzt, aber erlauben im Hintergrund Logik. (Auto-Properties, wie oben `Name`, generieren automatisch ein privates Feld). Properties können schreibgeschützt (`get` ohne `set` oder mit privatem set) sein. Ab C# 9 kann ein Setter als `init` deklariert werden, wodurch der Wert nur während der Objekterstellung gesetzt werden kann (z.B. durch Objektinitialisierer) – praktisch für Immutability (siehe *Records*).
    * *Konstruktoren* sind spezielle Methoden zur Initialisierung neuer Objekte (`public Person(string n) { ... }`). Wenn kein eigener Konstruktor definiert wird, hat die Klasse einen parameterlosen Default-Konstruktor. Konstruktoren können überladen werden (mehrere mit unterschiedlichen Parametern). Mit `static Person()` kann man einen statischen Konstruktor definieren, der einmalig vor der ersten Nutzung der Klasse ausgeführt wird (typisch für statische Initialisierungen).
    * *Finalizer* (\~Destruktor): `~ClassName() { ... }` kann definiert werden, um aufzuräumen, wenn das Objekt vom GC gesammelt wird. In .NET ist das selten nötig (lieber IDisposable/using nutzen). Finalizer werden nicht deterministisch ausgeführt und belasten die GC-Performance, daher sparsam einsetzen. Nur Klassen (Referenztypen) können Finalizer haben, Structs nicht.
    * *Verschachtelte Typen:* Innerhalb einer Klasse können weitere Klassen, Structs, Enums etc. definiert sein (als *nested types*). Diese verhalten sich wie static Member der äußeren Klasse (haben Zugriff auf private statische Mitglieder).
    * *Zugriffsmodifier:* `public`, `private`, `protected` (nur in Klasse und abgeleiteten Klassen zugreifbar), `internal` (nur im selben Assembly sichtbar), sowie Kombinationen `protected internal` und seit C# 7.2 `private protected`. Standardzugriff für Klassen-Member ist `private`, wenn nichts angegeben.
    * *`this`-Verweis:* Innerhalb einer Instanzmethode referenziert `this` das aktuelle Objekt. Bei statischen Methoden gibt es kein `this`. `base` verweist auf die Basisklassen-Implementationen (z.B. `base.ToString()` ruft die geerbte Version auf, oder im Konstruktor `base(...)` um Basiskonstruktor aufzurufen).

  * **Vererbung und Polymorphie:** Wenn Klasse B von A erbt, gilt B als *spezialisierte* A. B hat Zugriff auf `protected` Member von A. Methoden können `virtual` in A definiert werden und in B mit `override` überschrieben werden, um spezifischeres Verhalten bereitzustellen. Ein override muss denselben Signatur haben. Klassen ohne override einer geerbten virtuellen Methode erben standardmäßig die Implementierung von A. Man kann eine override-Methode in einer Zwischensubklasse auch mit `sealed override` wieder versiegeln, damit weitere abgeleitete sie nicht mehr ändern können. Polymorphie: Eine Variable vom Typ der Basisklasse kann zur Laufzeit ein Objekt der abgeleiteten Klasse halten und die passende override-Methode wird aufgerufen (Dynamic Dispatch). Beispiel:

    ```csharp
    Human h = new Person("Alice");
    Console.WriteLine(h.ToString()); // ruft Person.ToString() auf, falls ToString virtuell overridebar gemacht wurde
    ```

    Standard-Methoden in Object wie `ToString`, `Equals`, `GetHashCode` sind virtual und können override werden.

  * **Partial Classes:** Mit dem Keyword `partial` kann man die Definition einer Klasse auf mehrere Dateien aufteilen. Alle Teile müssen `partial class ClassName` deklarieren. Der Compiler fügt sie zusammen. Dies ist nützlich bei auto-generiertem Code (Designer-Code getrennt von Benutzer-Code). Partial kann auch bei Structs, Interfaces und Methoden verwendet werden. Bei partial method (in partial class) kann man eine Methodensignatur deklarieren mit `partial void DoSomething();` und diese in anderer Datei definieren. Wenn keine Implementierung gegeben wird, wird der Aufruf zur Compilezeit entfernt (kein Fehler) – partial methods müssen `void` sein und keine out-Parameter, etc., und sie sind implizit private.

* Ein **Struct** (`struct`) definiert einen *Werttyp*. Struct-Instanzen werden nicht auf dem Heap referenziert, sondern *direkt* dort gespeichert, wo sie deklariert werden (z.B. auf dem Stack als lokale Variable, oder inline als Feld innerhalb eines Objekts). Sie werden bei Zuweisungen *by value* kopiert. Structs eignen sich für kleine Datengrößen, wo die Kopierkosten gering sind, oder wenn viele davon in Arrays gehalten werden (Speicherlokalität). Beispiele für eingebaute Structs: `int`, `double`, `DateTime`, `Guid`, auch `Nullable<T>` ist ein Struct.

  Structs in C# ähneln Klassen in Syntax, aber es gibt einige wichtige Unterschiede:

  * Structs können Felder, Methoden, Properties, Operatoren, Nested Types definieren, **aber keine** expliziten Vererbungshierarchien. Ein Struct kann keine andere Klasse oder struct erben (alle Structs erben implizit von `System.ValueType` und damit von `object`). Sie können jedoch Interfaces implementieren.
  * Structs sind *sealed* per se – man kann nicht von einem struct erben. Daher gibt es in Structs auch kein `virtual`/`override` (abgesehen von den Methoden, die von `System.ValueType`/`Object` kommen, die man *überschreiben* kann – z.B. `ToString`, `Equals`, `GetHashCode` kann man in Structs neu definieren).
  * Felder in einem Struct dürfen nicht mit Initialisierern versehen werden (außer `const` und `static` Felder). Initialisierungen müssen im (parameterlosen) Konstruktor erfolgen. Allerdings *muss* ein struct-Konstruktor alle Felder vollständig initialisieren, bevor er endet.
  * Ein struct hat immer einen impliziten *parameterlosen* Standardkonstruktor, der alle Felder auf `default` setzt – dieser Standardkonstruktor kann vom Benutzer **nicht** überschrieben oder entfernt werden (bis C# 10: ab C# 10 darf man optional einen eigenen parameterlosen Konstruktor definieren, um einen anderen Defaultzustand herzustellen, aber der muss ebenfalls alle Felder setzen). Wenn man irgendeinen Konstruktor definiert, muss man in allen Konstruktoren alle Felder zuweisen, sonst Fehler. Beispiel:

    ```csharp
    public struct Point {
        public int X, Y;
        public Point(int x, int y) {
            X = x;
            Y = y;
        }
        // Parameterloser Konstruktor (ab C# 10 erlaubt):
        public Point() {
            X = Y = -1;
        }
    }
    ```

    Ohne definierten Konstruktor könnte man `new Point()` schreiben und bekäme (0,0).
  * Structs können instanziiert werden mit oder ohne `new`. `Point p;` deklariert eine struct-Variable, aber ohne new ist sie nicht initialisiert (alle Felder werden zwar auf 0 gesetzt – für lokale Variablen verlangt der Compiler aber, dass man sie vor Nutzung initialisiert). `Point p = new Point(2,3);` ruft den Konstruktor. Man kann auch `Point q = default;` schreiben (alle Felder default).
  * Zuweisung `p = q;` kopiert bei Structs das gesamte Datenpaket (`p` hat danach eigene Kopie). Bei Klassen würde nur die Referenz kopiert und beide auf dasselbe Objekt zeigen.
  * Structs können keine Finalizer definieren. Sie können auch kein `protected`-Mitglied haben (macht keinen Sinn ohne Vererbung).
  * Boxing: Wenn ein struct als `object` oder Interface behandelt wird, wird es *geboxt*, d.h. in ein Heap-Objekt kopiert. Exzessives Boxing kann Performance kosten, daher sollte man z.B. keine struct in Collections stecken, die als object arbeiten (besser generische Collections verwenden, die T als Werttyp behandeln, um Boxing zu vermeiden).
  * Structs können *`readonly`* deklariert werden (ab C# 7.2). Dann sind alle ihre Felder readonly, d.h. nach Konstruktion unveränderlich. Das verbessert ggf. Performance, weil der Compiler defensive Kopien vermeiden kann. Auch einzelne Felder eines Struct können `readonly` sein. Bei `readonly struct` sollten alle mutierenden Methoden als `readonly` markiert werden – ansonsten führt Aufruf auf einem readonly-Struct zu einer Kopie (da in C# das `this` bei struct-Methoden als ref übergeben wird, aber wenn `this` als readonly vorliegt, wird es als Kopie übergeben, damit die Originaldaten nicht modifiziert werden).
  * In struct-Methoden ist zu beachten: Der `this`-Parameter ist standardmäßig *by value*. D.h. innerhalb einer Instanzmethode hat man eine Kopie der Struct-Daten, wenn die Methode nicht als `ref` deklariert wurde. Dadurch bewirkt Änderungen an `this` in der Methode nicht, was man vielleicht erwartet. Als Schutz sind in C# struct-Instanzmethoden `this` als *readonly* behandelt, wenn die Methode selbst als `readonly` markiert ist oder auf einem readonly-Struct aufgerufen wird. So gibt der Compiler einen Fehler, falls man versucht in einer solchen Methode Felder zu ändern (weil das ja ins Leere gehen würde auf einer Kopie). Um das zu vermeiden, sollte man Mutator-Methoden nicht in `readonly`-Kontext aufrufen oder das struct per Referenz übergeben.

**Struct vs. Class zusammengefasst:** Structs sind wertsemantisch, haben keinen GC-Overhead pro Objekt (können auf dem Stack liegen oder inline in Arrays ohne Indirektion) und vermeiden Null (außer Nullable). Sie sind gut für kleine Datengruppen, bei denen Identität unwichtig ist und man viele Instanzen braucht (z.B. komplexe Zahl, Koordinate, Color-Wert). Klassen sind referenzsemantisch – mehrere Referenzen können auf dasselbe Objekt zeigen, eignen sich also wenn man Objektidentität/Teilen braucht oder die Daten groß sind (um Kopieraufwand zu vermeiden). Bei zu großen Structs kann es ineffizient werden, sie ständig zu kopieren. Eine Faustregel: Structs sollten nicht viel größer als \~16 Bytes sein und Wert-artig genutzt werden. Natürlich gibt es Ausnahmen (z.B. `System.Decimal` ist 16 bytes, `System.Numerics.BigInteger` oder `Guid` sind größer).

**ref struct:** Es gibt eine besondere Struct-Variante mit dem Modifier `ref` in der Definition (z.B. `public ref struct Span<T> { ... }`). *Ref structs* sind Stack-beschränkt – Instanzen dürfen nicht auf dem Heap landen. Dadurch gelten Einschränkungen: Ein `ref struct` kann nicht als Feld in einer Klasse oder normalen struct gespeichert werden, nicht in einem `object` gecastet (kein Boxing), nicht in Array gelegt werden, nicht als generisches Argument verwendet werden (außer der Typparameter ist explizit auf `allows ref struct` eingeschränkt, C# 13 Feature), und bis C# 13 konnte man sie nicht in Async-Methoden oder Iteratoren verwenden (ab C# 13 eingeschränkt möglich mit bestimmten Regeln). Beispiel eines ref struct ist `Span<T>`, welches einen Bereich von Speicher repräsentiert. Ref structs ermöglichen High-Performance Code ohne GC-Allokation, sind aber von der Lebensdauer ans Stack gebunden, weshalb der Compiler strikt überwacht, dass sie nicht aus ihrem Gültigkeitsbereich hinausleben (z.B. nicht als return zurückgeben, außer vielleicht als `ref` return bei gleicher Lebensdauer etc.). Für Alltagsentwickler tauchen ref structs selten direkt auf, eher im Framework (Span, ReadOnlySpan, etc.).

**Strukturgröße & Layout:** Standardmäßig kann der CLR den Speicher von Klassen/Structs optimal anordnen. Mit `[StructLayout]`-Attribute kann man bei Bedarf ein festes Layout erzwingen, etwa für P/Invoke-Szenarien (sequentiell oder explizit). In C# kann man auch *fixed size buffer* Felder in unsafe context definieren (siehe `fixed` in Unsafe), um z.B. ein Inline-Array in einem struct zu haben.

## Schnittstellen (Interfaces)

Ein **Interface** definiert einen Vertrag von Methoden, Properties, Ereignissen oder Indexern, die eine Klasse oder ein Struct erfüllen (implementieren) muss. Interfaces enthalten **keine** Implementierungsdetails (bis C# 7.3 zumindest) – sie sind rein abstrakt. Eine Klasse/Struct kann mehrere Interfaces implementieren (C# erlaubt damit quasi *Mehrfachvererbung* auf Interface-Ebene). Beispiel:

```csharp
public interface IPlayable 
{
    void Play();
    void Pause();
    bool IsPlaying { get; }
}
```

Hier verlangt `IPlayable`, dass ein implementierender Typ die Methoden `Play()`, `Pause()` und die Property `IsPlaying` bereitstellt. Interfaces haben per se keine Felder (bis auf `const`-Felder, die als Konstanten zulässig sind). In C# ≤7 konnten Interfaces auch keine Implementierungen haben – jede Methode war abstrakt (ohne Körper). Die Mitglieder sind implizit public (andere Zugriffsmodifizierer waren bis C# 8 nicht erlaubt).

**Implementierung:** Eine Klasse implementiert ein Interface, indem es beim Klassennamen angegeben wird (`class MyPlayer : IPlayable, IOtherInterface`). Anschließend muss die Klasse alle Interface-Mitglieder mit passender Signatur als `public` definieren. Beispiel:

```csharp
public class MusicPlayer : IPlayable 
{
    public bool IsPlaying { get; private set; }
    public void Play() { /* ... */ IsPlaying = true; }
    public void Pause() { /* ... */ IsPlaying = false; }
}
```

Nun kann man ein `MusicPlayer`-Objekt auch durch die Linse des Interfaces betrachten:

```csharp
IPlayable player = new MusicPlayer();
player.Play();              // Ruft MusicPlayer.Play() via Interface
Console.WriteLine(player.IsPlaying);  // True
```

Interfaces ermöglichen so Polymorphie ohne Klassenvererbung (z.B. unterschiedliche Klassen können IPlayable implementieren – etwa MusicPlayer, VideoPlayer – und vom Aufrufer einheitlich behandelt werden).

**Mehrfachimplementierung & Namenskonflikte:** Wenn zwei Interfaces dieselbe Methodensignatur fordern, ist das i.d.R. kein Problem – die Klasse implementiert sie einmal. Sollten Signaturen kollidieren oder man will gezielt *verschiedene* Implementierungen, kann man *explizite Interface-Implementierungen* verwenden. Dabei wird die Methodensignatur mit Interface-Namen qualifiziert: `void IPlayable.Play() { ... }`. Eine explizite Implementierung ist nicht public (sie ist nur über Interface-Variable aufrufbar). Sie erlaubt, z.B. zwei Interfaces mit gleicher Methode getrennt zu bedienen. Normalerweise vermeidet man Namenskonflikte aber.

**Standard-Implementierungen (C# 8)**: Ab **C# 8.0** dürfen Interfaces auch *Default Implementations* bereitstellen. Das heißt, eine Interface-Methode kann einen Rumpf haben (wie in Java 8 "default methods"). Klassen, die das Interface implementieren, können diese Methoden entweder überschreiben (in C# sagt man "implementieren" – aber tatsächlich können sie *optional* definieren) oder einfach die Standardimplementierung vom Interface "erben". Wichtig: Interfaces mit Implementierung sind immer noch keine Klassen; eine Klasse *erbt* die Implementierungen nicht im eigentlichen Sinne (die Methoden tauchen nicht als class members auf), aber wenn über das Interface aufgerufen, wird die default Implementation genutzt. Beispiel:

```csharp
public interface ILogger 
{
    void Log(string msg) { Console.WriteLine(msg); }  // default implementation
}
public class MyLogger : ILogger 
{
    // Keine eigene Log-Methode definiert -> benutzt die vom Interface
}
...
ILogger log = new MyLogger();
log.Log("Test");  // schreibt "Test", via ILogger.Log default body
// MyLogger-Instanz hat aber keine Log-Methode im Klassenvertrag:
MyLogger obj = new MyLogger();
// obj.Log("x"); // Compilerfehler: MyLogger hat keine Log-Member
```

Man sieht: `obj.Log("x")` geht nicht (die Klasse hat keine eigene `Log`-Methode), aber castet man `obj` zu `ILogger`, kann man `Log` aufrufen und es läuft die Interface-Implementierung. Das mag etwas kontraintuitiv sein. Man kann eine Klasse aber trotzdem eine eigene Implementierung geben, indem man die Methode normal (ohne override, da Interface keine virtuellen Klassenmethoden hat) definiert. Das Interface-Standard-Impl fungiert quasi wie eine *virtuelle* Methode, die von der Klasse übersteuert werden kann, aber es wird mit *expliziter Interface-Implementierung* überschrieben (C# fordert die Syntax `void ILogger.Log(string msg) { ... }` oder ab C# 8 darf man auch mit dem neuen `override` in Interfaces arbeiten wenn als virtual deklariert – das Detail führt hier zu weit).

Interfaces können seit C# 8 auch *statische* Member enthalten (z.B. `static method`, `static property`, sogar static fields und static constructor) und auch *private* Helfermethoden (instanz oder static), die nur innerhalb des Interface als Helfer für Default-Methoden genutzt werden. Außerdem gibt es seit C# 11 *abstrakte statische* Methoden in Interfaces (Stichwort Generic Math), womit man von generischen Typen bestimmte statische Operatoren/Methoden verlangen kann.

**Zusammenfassung:** Interfaces definieren *Was* ein Typ können muss (Methodensignaturen etc.), aber nicht *Wie* – bis auf neue Defaultmethoden-Fähigkeit, die als opt. Voreinstellung dienen. Sie ermöglichen polymorphes Verhalten über verschiedene, nicht unbedingt verwandte Klassen hinweg. Eine Klasse kann mehrere Interfaces implementieren (im Gegensatz zu nur einer Basisklasse), was flexible Designs erlaubt (Beispiel: `class Button : Control, IClickable, IDraggable` – erbt von Control und erfüllt zwei Fähigkeiten-Schnittstellen).

## Aufzählungstypen (Enums)

Ein **Enum** (Aufzählungstyp) ist ein Werttyp, der eine Menge benannter Konstanten definiert. Beispielsweise:

```csharp
public enum DayOfWeek { Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday }
```

Hier repräsentiert `DayOfWeek` sieben mögliche Werte. Intern sind Enums ganzzahlig – standardmäßig vom Typ `int` (32-bit). Die Mitglieder `Monday`...`Sunday` erhalten automatisch die Werte 0,1,...6 (sofern nicht anders angegeben). Man kann den zugrundeliegenden Typ ändern, indem man nach dem Enum-Namen `: byte` oder `: long` etc. angibt – erlaubt sind alle integralen Typen außer `char` (typischerweise nimmt man den kleinsten ausreichend großen Typ). Beispiel: `enum ByteEnum : byte { A, B, C }` hat A=0, B=1, C=2 vom Typ Byte.

Enums werden häufig für Zustände, Optionen oder Flags verwendet. Man kann einem Enum-Mitglied explizit einen Wert zuweisen, z.B.:

```csharp
enum ErrorCode { None=0, Unknown=1, ConnectionLost=100, OutOfMemory=200 }
```

Nicht gesetzte Elemente werden aufsteigend weitergezählt (hier würde nach Unknown=1 das nächste `ConnectionLost=100`, dann `OutOfMemory=200`, die Lücken bleiben unbenutzt).

**Verwendung:** Man deklariert Variablen vom Enum-Typ und weist einen der definierten Werte zu:

```csharp
DayOfWeek today = DayOfWeek.Friday;
if(today == DayOfWeek.Friday) { ... }
```

Enums können mittels Cast in ihre zugrundeliegende Zahl umgewandelt werden und umgekehrt. Beispiel: `(int)DayOfWeek.Wednesday` ergibt 2; oder `DayOfWeek d = (DayOfWeek)2;` ergibt `DayOfWeek.Wednesday`. **Achtung:** Der Cast von int zu Enum *funktioniert immer*, auch wenn die Zahl kein definierter Wert ist. Z.B. `(DayOfWeek)77` ergibt ein `DayOfWeek` mit internem Wert 77, den man auch zuweisen kann; es ist nur kein benanntes Mitglied. Man sollte also vorsichtig sein, und ggf. prüfen `Enum.IsDefined(typeof(DayOfWeek), value)` bevor man blind castet.

Man kann die `System.Enum`-Hilfsklasse nutzen (alle Enums erben von System.Enum). Z.B. `Enum.GetNames(typeof(DayOfWeek))` gibt `string[] { "Monday","Tuesday",... }`. Oder `Enum.TryParse<DayOfWeek>("Friday", out var day)` um aus einem String einen Enum-Wert zu bekommen.

**Flags:** Oft möchte man Enums verwenden, um Bitfelder darzustellen, wo Werte kombiniert werden können (z.B. `[Flags] public enum FileAccess { Read=1, Write=2, ReadWrite = Read | Write }`). Das `[Flags]`-Attribut bewirkt, dass z.B. die `ToString()`-Ausgabe mehrere gesetzte Bits kombiniert anzeigt. Im obigen FileAccess-Beispiel hätte `FileAccess.Read | FileAccess.Write` den kombinierten Wert 3 und `ToString()` würde "Read, Write" ausgeben. Wichtig ist, dass die Enum-Mitglieder entsprechend als einzelne Bits definiert sind (1,2,4,8,...). Das Flags-Attribut ist rein dekorativ (für bessere String-Darstellung und eventuell Tools), die eigentliche Bitlogik funktioniert auch ohne das Attribut, sofern man die Werte manuell kombiniert. Beispiel:

```csharp
[Flags]
enum Days { None=0, Monday=1, Tuesday=2, Wednesday=4, Thursday=8, Friday=16, Saturday=32, Sunday=64, Weekend = Saturday | Sunday }
...
Days meeting = Days.Monday | Days.Wednesday | Days.Friday;
Console.WriteLine(meeting);  
// Ausgabe (Dank [Flags]): "Monday, Wednesday, Friday"
bool isTuesday = (meeting & Days.Tuesday) == Days.Tuesday;  // prüfen ob Dienstag-Flag gesetzt
```

Hier sieht man: Durch Bitmaskierung mit `&` kann man Flags ein-/ausschalten und abfragen.

Enums sind per Standard `int` und 0 ist der Default-Wert (ungültig, falls nicht definiert, außer man definiert z.B. "None = 0"). Es ist gute Praxis, einen expliziten `0`-Wert im Enum zu haben, besonders für Flags (z.B. "None = 0" oder "Unknown = 0"), damit uninitialisierte oder standard-reset Werte sinnvoll interpretierbar sind.

Enums unterstützen keine Methoden (abgesehen von den geerbten von System.Enum), aber man kann in neueren C# static Extension Methods definieren oder static helper method in `partial enum` (C#9 erlaubt `partial` methods und static in Enums glaube ich nicht, nur Klassen können partial sein, Enums nicht).

In C# 7.3 wurde der generische *Enum Constraint* eingeführt: man kann generische Methoden beschränken mit `where T: System.Enum`. Das erlaubt in generischen Code Enums als spezielle Gruppe zu behandeln (z.B. eine Methode, die nur für Enums Sinn ergibt). Intern zählt Enum als struct (da ValueType), daher erfüllte schon `where T: struct`, aber mit `where T: Enum` hat man es klarer und kann auf Enum-Hilfsmethoden zugreifen.

## Records

**Records** sind eine Sprachfunktion eingeführt in C# 9, die vor allem *immutable data classes* vereinfachen. Ein `record` kann man sich als eine spezielle Form einer Klasse (oder Struct) vorstellen, die vor allem folgende auszeichnet:

* **Value-basierte Gleichheit:** Zwei Record-Instanzen werden durch Inhalt verglichen (alle nicht-statischen Felder/Properties) anstatt durch Referenz. Das bedeutet, `record Person(string Name)` – zwei Person-Objekte mit demselben Name gelten als `Equals` und haben denselben HashCode, auch wenn es zwei verschiedene Instanzen sind. Bei normalen Klassen würde `Equals` (ohne Override) Referenzgleichheit prüfen. Der Compiler generiert in Records automatisch `Equals`/`GetHashCode`-Overrides, die die Inhalte vergleichen. (Für Records, die abgeleitete Klassen sind, gibt es ein etwas komplexeres EqualityContract-Konzept, damit Gleichheit nur bei identischem Laufzeittyp gilt – sprich, ein Record vergleicht nicht gleich zu einem geerbten Record-Typ selbst wenn Felder gleich wären.)

* **Unveränderlichkeit (Immutable) Unterstützung:** Records sind primär gedacht mit `init`-only Properties, sodass nach Erstellung die Daten nicht mehr verändert werden. Der Compiler bietet eine verkürzte Syntax mit **Primärkonstruktor**: `public record Person(string FirstName, string LastName);`. Dies erzeugt automatisch öffentliche init-only Auto-Properties `FirstName` und `LastName` und einen Konstruktor, der diese setzt. So hat man mit einer Zeile eine Datenklasse definiert. Alternativ kann man das Record auch in ausführlicher Form schreiben (siehe Beispiele unten).

* **`with`-Ausdruck für *nondestructive mutation*:** Wie oben im Pattern-Abschnitt erwähnt, generiert der Compiler für Records einen *Kopierkonstruktor* und ermöglicht die `with`-Syntax. Z.B.:

  ```csharp
  var p1 = new Person("Alice", "Smith");
  var p2 = p1 with { LastName = "Johnson" };
  ```

  Hier wird `p2` eine Kopie von `p1` aber mit geändertem Nachnamen. `p1` bleibt unverändert. Unter der Haube erzeugt der Compiler in `Person` eine Methode `public Person With(... )` die neue Instanz erstellt und differierende Properties ersetzt, aber man nutzt es syntaktisch über `with`.

* **Positions- und Dekonstruktor:** Für Records mit Primärkonstruktor gibt es automatischen *Deconstruct*-Methoden, sodass man z.B. `var (f,l) = person;` schreiben kann, um die FirstName/LastName rauszuholen. (Ähnlich wie man das auch bei Tuple oder von Hand implementierten Deconstruct kennt.)

* **ToString override:** Der Compiler generiert eine `ToString()`-Methode, die den Recordinhalt formatiert ausgibt, etwa `Person { FirstName = Alice, LastName = Smith }`. Das erleichtert Debugging/Logging.

* **`record class` vs `record struct`:** Standard ist `record` ohne Keyword bedeutet ein *Referenztyp* (eine Klasse). Man kann aber auch `record struct` definieren (eingeführt in C# 10) – das ist dann ein ValueType-Record, d.h. hat Wertsemantik *und* Werttypverhalten (wird by value kopiert, etc.). Ein `record struct` verhält sich beim Gleichheitsvergleich wie ein normaler Struct (also bitweiser Vergleich über ValueType.Equals per Reflektion), außer man überschreibt oder lässt den Compiler Synthese? (Tatsächlich generiert der Compiler *nicht* denselben equals für record struct wie record class – bei record struct entspricht equals dem von ValueType, d.h. vergleicht per Reflection alle Felder, was ineffizient ist, aber in Comments lesen wir, dass sie es trotzdem tun? => aus dem Snippet: "For records, the implementation is compiler synthesized und nutzt declared data members", es sagt aber in L13-L17, bei record struct ist equality wie normal struct, Implementation in ValueType.Equals via Reflection.)

  Jedenfalls: record struct bieten ValueType mit ähnlichem Komfort (auto-init properties etc.), aber oft verwendet man record class.

* **Vererbung:** Records können eine Vererbungshierarchie bilden, aber alle beteiligten Typen müssen `record` sein (eine record class kann eine andere record class erben). Bei Gleichheit wird – wie erwähnt – auf *gleichen Laufzeittyp* bestanden, d.h. ein Base-Record und ein Derived-Record sind nie Equal, selbst wenn alle Felder identisch sind. Das verhindert, dass man z.B. Base-Eigenschaften vergleicht ohne die Derived-Felder. Der Compiler erzeugt hierfür eine geschützte Eigenschaft `EqualityContract` in jedem Record, die benutzt wird, um den Typ zu prüfen. Vererbung mit Records ist fortgeschritten, oft verwendet man Records eher als sealed (man kann ein record auch `sealed` deklarieren, dann spart sich der Compiler die Type-Check-Konstrukte in Equals).

* **Mutabilität:** Obwohl primär für Immutables gedacht, können Records auch mit veränderlichen Properties genutzt werden (z.B. `public string Name { get; set; }`). Das obige Snippet zeigt sogar ein Beispiel mit mutable record. In dem Fall ist die Gleichheits-Implementierung trotzdem anfangs vom Konstruktorzustand abhängig; ändert man später eine Property, ändert sich damit ggf. das Equals-Verhalten (zwei Records, die equal waren, könnten auseinanderdriften wenn man einen davon modifiziert). Das ist etwas, wovon man sich bewusst sein muss. Darum der Hinweis in den Docs: "While records can be mutable, they're primarily intended for immutable data models.". Also es *geht*, aber vorsichtig einsetzen. Man kann z.B. `required` Auto-Properties nutzen (C# 11 Feature): `public required string Name { get; init; }` bedeutet, dass beim Initialisieren (Konstruktor oder Objektinitializer) ein Wert gesetzt werden muss, andernfalls gibt's einen Fehler. Das hilft sicherzustellen, dass alle nötigen Felder belegt sind.

**Record-Beispiele:**

* Kurznotation (positional record):

  ```csharp
  public record Person(string FirstName, string LastName);
  ```

  Compiler erstellt: Properties FirstName, LastName (init-only), einen Konstruktor Person(string, string) der die Properties setzt, `Deconstruct(out string, out string)`, `Equals`, `GetHashCode`, `ToString` etc..

* Ausführliche Notation:

  ```csharp
  public record Person 
  {
      public string FirstName { get; init; }
      public string LastName { get; init; }
      public Person(string first, string last) {
          FirstName = first;
          LastName = last;
      }
      // Weitere Mitglieder möglich
  }
  ```

  Dies ist funktional ähnlich, nur ohne auto-generierte Deconstruct/ToString (es sei denn, man ruft `ToString` nicht überschrieben, dann generiert der Compiler trotzdem ein override). Man kann auch den Primärkonstruktor mit Body kombinieren:

  ```csharp
  public record Person(string FirstName, string LastName) 
  {
      public string FullName => $"{FirstName} {LastName}";
  }
  ```

  Hier wird FullName als abgeleitete Eigenschaft hinzugefügt; der Primärkonstruktor ist gegeben.

Records verhalten sich im Code ansonsten wie Klassen: Sie können Methoden haben, Interfaces implementieren, static Members, etc. Ihr entscheidender Vorteil ist die Boilerplate-Ersparnis für Datenklassen (Konstruktor, Equals, GetHashCode, ToString, Properties). So lassen sich z.B. DTOs, Konfigobjekte oder Ergebnis-Typen schnell definieren.

## Delegates und Events

**Delegates** sind Typen, die auf Methoden verweisen können – man kann sie sich als Typsichere Funktionszeiger vorstellen. Ein Delegate-Typ deklariert das Signaturschema der Methode(n), die er aufnehmen kann. Beispiel:

```csharp
public delegate int Transformer(int x);
```

Dies definiert einen Delegate-Typ namens `Transformer`, der Methoden mit einer `int -> int` Signatur repräsentieren kann. Man kann nun Variablen dieses Typs instantiieren, z.B.:

```csharp
int Square(int z) => z * z;
Transformer t = Square;               // Methode zuweisen
int result = t(3);                    // ruft Square(3) auf, result = 9
```

Man kann auch anonym eine Methode per Lambda zuweisen: `Transformer t = x => x * x;`. Delegates sind Klassen hinter den Kulissen (erben von `System.Delegate`). Man kann mit `new Transformer(Funktion)` auch explizit instanziieren, aber der Compiler erlaubt abgekürzt wie oben.

Delegates können mehrere Methoden enthalten (Multicast-Delegates). Man kann mittels `+=` mehrere Handler anhängen. Z.B.:

```csharp
Action actions = MethodA;
actions += MethodB;
actions();
```

`Action` ist ein vordefinierter Delegate (ohne Rückgabewert, beliebig viele Param). Nach dem `+=` enthält `actions` zwei Methoden. Beim Aufruf `actions()` werden *nacheinander* MethodA und MethodB ausgeführt (in Reihenfolge der Hinzufügung). Der Rückgabewert bei Multicast: Wenn der Delegate einen Rückgabewert hat, liefert er beim Aufruf den Wert des *letzten* aufgerufenen Handlers. (Für `void` Delegates wie Action stellt sich die Frage nicht.)

Man kann mit `-=` einen Handler vom Delegate entfernen. Achtung: Das Entfernen erfordert eine Referenz auf denselben Delegate oder eine gleicher Method/Kombination, ansonsten passiert nichts, falls der Handler nicht gefunden wird.

**Built-in Delegate Types:** Es gibt generische vordefinierte Delegates: `Action<T1,...,T16>` für Methoden ohne Rückgabe (bis 16 Parameter), `Func<T1,...,TResult>` für Methoden mit Rückgabe, `Predicate<T>` für Func\<T,bool> Kurzform. Z.B. `Func<int,string>` repräsentiert Methode mit int->string. Es gibt auch `EventHandler`/`EventHandler<TEventArgs>` Konventionen, die für Events genutzt werden (s.u.).

**Delegates als Callback:** Delegates werden benutzt, um Funktionen als Parameter zu übergeben (z.B. Sortierregeln, LINQ Funktonen) oder Ereignisse zu modellieren. Beispiel: In `List<T>.Sort(Comparison<T> comparison)` erwartet `Comparison<T>` ist ein Delegate `delegate int Comparison<T>(T x, T y)` (negativ,0,positiv je nach Sortierung). Man kann dort z.B. `myList.Sort((a,b) => a.Age.CompareTo(b.Age));` schreiben – hier wird ein Delegate via Lambda übergeben.

**Events:** Ein **Event** in C# ist ein spezieller Mechanismus, der auf Delegates aufbaut, um das *Beobachter/Publisher-Subscriber*-Muster zu implementieren. Man deklariert ein Event in einer Klasse mit dem Keyword `event` vor einem Delegatetyp. Beispiel:

```csharp
public class Button 
{
    public event EventHandler Click;
    protected void OnClick() {
        if(Click != null)
            Click(this, EventArgs.Empty);
    }
}
```

Hier hat `Button` ein Event namens `Click` vom Typ `EventHandler` (vordef. Delegate `delegate void EventHandler(object sender, EventArgs e)`). Andere Objekte können sich darauf registrieren:

```csharp
Button btn = new Button();
btn.Click += Button_Clicked;    // Subscribe
...
void Button_Clicked(object sender, EventArgs e) {
    Console.WriteLine("Button was clicked.");
}
```

Mehrere Abonnenten können sich mit `+=` an das Event hängen. Wenn das Event ausgelöst (gefeuert) wird – hier via `OnClick()` – werden alle Abonnenten-Methoden aufgerufen.

**Implementierungsdetails:** Ein `event`-Feld verhält sich ähnlich wie ein Multicast-Delegate-Feld, allerdings erzwingt der C#-Compiler Kapselung:

* Von *außen* (andere Klassen) kann man nur `+=` oder `-=` auf ein Event ausführen. Direkt aufrufen (invoken) oder mit `=` überschreiben ist nicht erlaubt (Compilerfehler). So kann kein externer Code das Event versehentlich löschen oder auslösen – das darf nur die deklarierende Klasse selbst.
* Innerhalb der definierenden Klasse kann man das Event behandeln wie ein Delegate vom angegebenen Typ. D.h. man kann im obigen Beispiel `Click(this, EventArgs.Empty)` aufrufen, aber viele checken vorher `if (Click != null)` (oder nutzen `Click?.Invoke(...)` in neuem Syntax), um NullReferenceException zu vermeiden, falls keiner aboniert hat.
* `event` ist also eine *Schutzschicht* um Delegates, speziell fürs Observer-Pattern.

**EventHandler und EventArgs:** Konventionell definieren Events oft Delegates vom Typ `EventHandler<T>` wo T von EventArgs erbt und Infos zum Ereignis enthält. Z.B. `public event EventHandler<MouseEventArgs> MouseMoved;`. Der Sender (object sender) ist fast immer der `this` der auslösenden Klasse. In .NET gibt es viele vordef. EventArgs-Subklassen (z.B. MouseEventArgs mit X,Y Buttons etc.). Für Fälle ohne extra Info nimmt man `EventArgs.Empty`. `EventHandler` ist einfach `delegate void EventHandler(object, EventArgs)` – also wie EventHandler<T> nur mit EventArgs fix.

**Beispiel mit eigenem Delegate:** Man kann auch eigene Delegatetypen für Events definieren:

```csharp
public delegate void ThresholdReachedEventHandler(object sender, ThresholdReachedEventArgs e);
public class Counter {
    public event ThresholdReachedEventHandler ThresholdReached;
    ...
    protected void OnThresholdReached(int threshold) {
        ThresholdReached?.Invoke(this, new ThresholdReachedEventArgs(threshold));
    }
}
```

Hier hat `ThresholdReachedEventArgs` als subclass von EventArgs evtl. nur ne property fürs threshold. Das Muster ist repetitiv; daher meist reicht EventHandler<T> aus.

**Events vs Delegates:** Warum Events, wenn Delegates eh ähnliches könnten? Hauptsächlich wegen *Kapselung*: Event-Eigentümer kontrolliert Aufruf und Abonnement. Der Unterschied:

* Wenn man `public Action Something;` (ein Delegate-Feld) macht, könnte Außenstehender `obj.Something = null;` alle Abonnenten kicken oder `obj.Something();` aufrufen – unsauber. Mit `public event Action Something;` verhindert C# solche Operationen von außen.
* Intern sind events oft Multi-Delegates. Implementation kann man auch *selbst* definieren mit `add` und `remove` Accessoren beim Event (ähnlich Property get/set). Das wird selten benötigt, außer man will z.B. Events anders speichern (z.B. in einer Dictionary, wie WPF es tut, um viele Events memory-effizient zu managen).

**Delegates in Kombination mit LINQ und Lambdas:** Delegates sind wesentliche Bausteine in LINQ-Methoden wie `List.Find(Predicate<T>)`, `Enumerable.Select(Func<TSource,TResult>)`, etc. Man nutzt dafür meistens Lambda-Ausdrücke an Ort und Stelle statt erst einen benannten Delegate zu definieren.

**Multithreading Events:** Bei Events muss man auf Thread-Sicherheit achten: `eventDel?.Invoke(...)` ist nicht atomar gegen unsubscribes auf anderem Thread. Best Practice: Lokale Kopie nehmen: `var handler = Click; if(handler != null) handler(this,e);`. Das verhindert NullRef falls zwischen Check und Invoke unsubscribed wurde. C# Event-Invoke-Syntax `Click?.Invoke(this,e)` erledigt intern diesen Thread-sicheren Copy-Check.

**Summary:** Delegates und Events ermöglichen *Inversion of Control*: Code kann an anderes Code Blöcke übergeben (Call-backs) oder ein System kann Abonnenten benachrichtigen (Events). Delegates sind flexibel (auch Rückgabewerte etc.), Events sind auf `void` Delegates (meist) ausgerichtet, weil ein Event mehrere Handler hat und typischerweise man deren Rückgaben nicht verwendet.

## Generics (Generische Typen und Methoden)

**Generics** erlauben die Definition von Klassen, Strukturen, Interfaces, Delegates oder Methoden mit *Typparametern*. Damit kann der gleiche Code mit verschiedenen Datentypen genutzt werden, ohne auf `object` und Casts ausweichen zu müssen (was unsicher und ineffizient wäre).

Ein **generischer Typ** wird durch spitze Klammern `<T>` (oder mehrere Typparameter `<T,U,...>`) gekennzeichnet. Beispiel einer generischen Klasse:

```csharp
public class Box<T> 
{
    private T content;
    public Box(T item) { content = item; }
    public T Content { get => content; set => content = value; }
}
```

Hier kann `T` für beliebige Typen stehen. Man *instanziiert* generische Klassen, indem man konkrete Typargumente angibt: `Box<int> intBox = new Box<int>(5);` oder `Box<string> strBox = new Box<string>("hello");`. Intern wird vom JIT-Compiler für jeden *ValueType* Typargument eine eigene maschinencode-spezialisierte Version erzeugt, für Referenztypen wird eine gemeinsame verwendet. Für den C#-Programmierer erscheint es, als hätte man beliebig viele Varianten der Klasse, ohne sie mehrfach schreiben zu müssen. Das bringt Typischerweise **Typensicherheit** und **Performance** (z.B. keine Boxing bei Werttypen) in Sammlungen und Algorithmen, die sonst auf object basieren müssten. Die .NET-Klassenbibliothek nutzt Generics intensiv (List<T>, Dictionary\<TKey,TValue>, Nullable<T>, Task<TResult> etc.).

Ein **generischer Method** ist ähnlich, hat aber Typparameter auf Methodenebene:

```csharp
public static T Max<T>(T a, T b) where T : IComparable<T>
{
    return (a.CompareTo(b) >= 0) ? a : b;
}
```

Hier wird `Max` für beliebige `T` definiert, aber mittels **Constraints** eingeschränkt (siehe `where`-Klausel): `where T : IComparable<T>` verlangt, dass der Typ T das Interface IComparable<T> implementiert (d.h. vergleichbar ist). Dadurch kann die Methode `CompareTo` auf a und b aufrufen. Man ruft die generische Methode auf, entweder lässt man den Compiler den Typ inferieren: `int m = Max(3,5);` – hier erkennt er T=int aus den Argumenten. Oder man gibt ihn explizit: `string longer = Max<string>("apple","pear");` (wobei hier inferieren auch ginge).

**Generische Constraints:** Diese `where`-Klauseln definieren Anforderungen an die Typparameter:

* `where T : struct` – T muss ein Werttyp sein (inkl. Nullable<T>). Bewirkt auch, dass T keinen Null-Wert hat (außer man nutzt Nullable). Oft verwendet, um z.B. mathematische Funktionen nur für Werttypen zuzulassen.
* `where T : class` – T muss ein Referenztyp sein. (In C# 8+ bedeutet das zusätzlich "nicht Nullable" im Hinblick auf NRT-Anmerkungen, es sei denn man schreibt `class?` im Constraint).
* `where T : unmanaged` – T muss ein *unverwalteter* Werttyp sein (also struct ohne Referenzfelder). Das impliziert `struct`. Mit diesem Constraint kann man z.B. generischen Code schreiben, der unsichere Pointerarithmetik auf T-Arrays macht, etc.
* `where T : new()` – T muss einen öffentlichen parameterlosen Konstruktor haben. Dadurch kann man in generischem Code `new T()` aufrufen. (Dieser Constraint kann nicht mit struct kombiniert werden, da struct eh paramlosen hat – ABER man darf nicht `where T: struct, new()`, weil `struct` impliziert bereits new() *kann*, aber es war eine Einschränkung, dass new() Constraint nicht mit struct kombiniert sein darf, ausser das hat sich geändert, doch \[56†L5-L11] sagt unmanaged impliziert struct, und new() kann nicht mit, oh \[56] snippet partial zeigt "cannot combine new with struct"? Actually \[56†L5-L11] shows: "The unmanaged constraint implies the struct constraint and can't be combined with either the struct or new() constraints." – also `unmanaged` darf man nicht zusammen mit struct/new schreiben, aber `struct` und `new()` zusammen ist erlaubt und gängig). Naja. Jedenfalls `where T: new()` oft zusammen mit class, um z.B. generisch Objekte erzeugen zu können.
* `where T : <base class>` – T muss von der angegebenen Klasse erben (oder die Klasse selbst sein). Z.B. `where T : Control` begrenzt T auf Control oder abgeleitete Klassen. Das erlaubt in generischem Code, auf Mitglieder von Control zuzugreifen.
* `where T : <interface>` – T muss dieses Interface implementieren (wie im Max-Beispiel). Man kann mehrere Interfaces angeben, kommasepariert.
* `where T : U` – Typparameter T muss ein Typ sein, der Typparameter U (einer anderen Typparameter oder sogar konkreter Typ an zweiter Stelle in der Parameterliste) erbt/implementiert. Etwa in einer Method `Foo<T,U> where T: U` – T muss Untertyp von U sein.
* `where T : notnull` – neu in C# 8, gibt an dass T keine null-Werte zulässt. Für Referenztypen heißt es NonNullable (behandelt T wie class mit Nullability disabled), für Werttypen hat es keinen Effekt außer disallow `T`=Nullable<U>. Ist hilfreich, um z.B. in generischen Code Null vergleichen zu können (`== null` ohne Warnung).
* `where T : scoped` (C# 11, advanced, zur Begrenzung von ref lifetimes – spezielle Thematik, wird hier ausgelassen).
* Spezielle Constraints: `where T : System.Enum` – T muss ein Enumtyp sein. `where T : Delegate` oder `MulticastDelegate` – T muss ein Delegate-Typ sein. Diese wurden in C# 7.3 eingeführt (Enum, Delegate, MulticastDelegate).
* In neueren Versionen: `where T : unmanaged` haben wir, `where T : nint` gibts nicht, aber man kann function pointers etc. egal.

Mehrere Constraints werden mit Komma getrennt: `where T : class, IMyInterface, new()`. Beachte: `new()` muss immer zuletzt stehen. `struct`/`class` sollten zuerst (manche Reihenfolgevorschriften).

**Variance (Kovarianz/Kontravarianz):** Betrifft generische Interfaces und Delegates mit *Vererbungsbeziehungen*. In generischen Typparametern kann man `out` oder `in` voranstellen (nur bei Interface/Delegate-Definition). Beispiel: `interface IEnumerable<out T>`. Das `out` besagt, dass IEnumerable<T> *kovariant* ist in T, d.h. ein `IEnumerable<string>` kann als `IEnumerable<object>` betrachtet werden, weil string zu object kovariant ist. Das funktioniert, weil IEnumerable nur T *ausgibt* (z.B. via GetEnumerator->Current vom Typ T, nie eine T *entgegen nimmt*). Umgekehrt gibt es `in` für Kontravarianz, z.B. `interface IComparer<in T>` – ein `IComparer<object>` kann auch `IComparer<string>` sein (weil Compare nimmt zwei T entgegen, und ein Comparer für object kann auch strings vergleichen). Kovarianz/Kontravarianz sind Spezialfälle und nur erlaubt, wenn bestimmte Voraussetzungen erfüllt sind (z.B. Typparameter nur als Rückgabetyp (out) bzw. nur als Parameter (in) in allen Membern). Die meisten .NET generischen Interfaces sind entsprechend markiert, z.B. `IEnumerable<out T>`, `IReadOnlyList<out T>`, `IComparer<in T>`, `Action<in T>` (Parameter), `Func<out TResult>`. Für eigene generische Interfaces kann man `out`/`in` an Typparams verwenden seit C# 4.

**Generische Delegates** analog: `public delegate TResult Converter<in TInput, out TResult>(TInput input);` – Variation. Oft nicht manuell nötig, man nutzt existierende Func/Action.

**Generische Constraints bei Klassen**: Generische Klassen können auch Constraints haben, z.B. `class Repository<T> where T: IEntity`.

**default(T)**: In generischem Code weiß man T nicht, aber man kann `default` als universellen Nullwert oder Standardwert nutzen. Für Referenztypen ist default = null, für Werttypen = 0/false/… Bits=0. Für Nullable<T> default = null. Ab C# 7.1 kann man einfach `default` literarisch schreiben, Compiler deduziert den Typ aus Kontext. Bsp: `T value = default;`.

**Typinferenz bei generischen Methoden**: Der Compiler ermittelt Typargumente aus den übergebenen Parametern oft automatisch, was Code vereinfacht (z.B. kein `Max<int>(3,5)` nötig, nur `Max(3,5)`). Bei Lambdas in generischen Methods wird's tricky, manchmal muss man `<Type>` angeben. In seltenen Fällen kann man Typargument nicht inferieren und muss es benennen.

**Praxis-Beispiele:**

* `List<T>` – generische Liste. Verwendet internal ein Array von T und vermeidet Boxing/unsafe.
* `Dictionary<TKey,TValue>`, `Queue<T>`, `Nullable<T>` etc.
* Generische Methoden: `Enumerable.ToList<T>(IEnumerable<T>)`, hier T wird aus Input bestimmt. Oder `Array.ConvertAll<TInput,TOutput>(TInput[], Converter<TInput,TOutput>)`, dort oft Lambdas.

**Constraints Best Practices:** So restriktiv wie nötig angeben, damit im Methodenkörper alles was man über T annehmen will auch vom Compiler erlaubt ist. Bsp: Arithmetik mit generischem T geht nicht, weil Operatoren nicht Constraintbar sind (C# 11 hat static abstract in interface to allow generic math: `where T : INumber<T>` etc. – neues Feature für .NET 7 numeric abstractions). Klassisch musste man Tricks machen (z.B. generische Addition war nicht ohne Boxing/IL hacks möglich). Nun mit `static abstract` methods in interfaces (C# 11) kann man generische Operatoren definieren.

**Performance:** Generics vermeiden Laufzeit-Casting, Polymorphismus erfolgt compilezeit (für ValueType pro Type separate code – was Codegröße erhöht aber Performance bringt; für RefType ein Code mit Typchecks). Das macht generische Collections *wesentlich schneller* als alte non-generic (ArrayList etc.), gerade bei Werttypen (kein Boxing).

**Weitere generische Features:**

* Man kann `typeof(T)` verwenden in generischen Klassen z.B. für Logging von Typparametern.
* `default` haben wir.
* Constrained calls: Der JIT kann bei `where T: struct` gewisse Optimierungen machen, direkter call statt via boxing virtueller call (z.B. calling object.ToString auf T wird T.ToString override via constraint auf ValueType trotzdem boxing, aber bei interface wie IComparable ruft JIT vtable an, egal).
* Sogar pointerlike: `stackalloc T[...]` in generics geht nur `where T: unmanaged`.

Generics sind ein sehr weites Feld; hier die wichtigsten Punkte: Typparameter erhöhen Codewiederverwendbarkeit und Typsicherheit, Constraints machen sie flexibel steuerbar, Variance löst Problem der Zuweisungskompatibilität. Für den Alltagsgebrauch nutzt man vor allem generische Collections und LINQ. Eigene generische Klassen schreibt man z.B. für Repositorien, Data Structures, etc.

## Attribute

**Attribute** sind zusätzliche Metadaten, die man an nahezu alle Programmkonstrukte anhängen kann (Assemblies, Klassen, Methoden, Properties, Parameter, etc.). In C# schreibt man Attribute in eckigen Klammern oberhalb der Deklaration, z.B.:

```csharp
[Obsolete("Use NewMethod instead")]
public void OldMethod() { ... }
```

Hier wird das `Obsolete`-Attribut angewendet, was dem Compiler signalisiert, beim Benutzen dieser Methode eine Warnung oder Fehler auszugeben (je nach Obsolete-Einstellungen). Attribute selbst sind Klassen, die von `System.Attribute` erben. Das Attribut `ObsoleteAttribute` ist z.B. so definiert und hat einen Konstruktor, der eine Message entgegennimmt und ein Named Parameter `IsError` optional.

**Anwendung von Attributen:** Viele .NET-APIs oder Tools werten Attribute aus:

* `[Obsolete]` vom Compiler,
* `[DllImport(...)]` um anzugeben, dass eine Methode aus einer unmanaged DLL kommt (Interoperabilität),
* `[TestMethod]` in Unit-Testing-Frameworks (der Test Runner reflektiert diese),
* `[Serializable]` markiert Klassen, die binär serialisierbar sind (für `BinaryFormatter`),
* `[XmlElement("Name")]` in XML-Serialisierung zur Konfiguration,
* `[HttpGet]` in ASP.NET Core Controllern, etc.

Attribute können Positionsparameter (über Konstruktor) und benannte Parameter (eigentlich Public Properties/Fields des Attribut-Klasse) erhalten. Z.B.:

```csharp
[MyAttr(123, Name="Test", Flag=true)]
class Sample { ... }
```

Hier ruft der Compiler den Konstruktor `MyAttr(int)` mit 123 auf, und setzt dann die Property `Name` und `Flag` falls vorhanden. Diese Informationen werden in die Assembly-Metadaten eingebettet und können zur Laufzeit via *Reflection* ausgelesen werden (z.B. `typeof(Sample).GetCustomAttributes(typeof(MyAttr), false)` liefert Instanzen des Attributes). Attribute sind also in erster Linie ein Mechanismus, um Zusatzinformationen im Code unterzubringen, die über Reflection oder vom Laufzeitsystem genutzt werden können.

**Definieren eigener Attribute:** Man schreibt eine Klasse, die von `System.Attribute` erbt. Konventionell enden Attribut-Klassen mit Suffix "Attribute", können aber ohne Suffix verwendet werden. Beispiel:

```csharp
public class AuthorAttribute : Attribute 
{
    public string Name { get; }
    public AuthorAttribute(string name) {
        Name = name;
    }
    public int Version { get; set; }  // Named parameter
}
```

Dieses Attribut kann man nun z.B. anwenden:

```csharp
[Author("Alice", Version = 2)]
class MyComponent { ... }
```

Man könnte später via Reflection ermitteln, wer Author ist:

```csharp
var attr = (AuthorAttribute)Attribute.GetCustomAttribute(typeof(MyComponent), typeof(AuthorAttribute));
Console.WriteLine(attr.Name + " v" + attr.Version);  // "Alice v2"
```

**AttributeTargets & Usage:** Standardmäßig kann ein Attribut an alles applied werden. Um es einzuschränken oder anderes Verhalten festzulegen, kann man es mit `[AttributeUsage]` dekorieren, z.B.:

```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct, AllowMultiple = false, Inherited = true)]
public class AuthorAttribute : Attribute { ... }
```

`AttributeTargets` ist ein Enum-Flag, man kombiniert Werte per `|`. Hier würde `AuthorAttribute` nur auf Klassen oder Structs erlaubt sein. `AllowMultiple=false` (Standard) bedeutet, man darf das Attribut nicht mehrfach auf das gleiche Element anwenden. `Inherited=true` (Standard false für eigene) bedeutet, wenn eine Klasse abgeleitet ist von einer mit Attribut, dann wird das Attribut auch als an der abgeleiteten Klasse vorhanden betrachtet (Reflection GetCustomAttributes kann vererbte Attribute einbeziehen). Manche Attribute wie `[Obsolete]` haben `Inherited=false` (wird nicht auf Subclass geerbt), aber z.B. `[AttributeUsage(Inherited=true)]` ist gängig bei z.B. \[Authorize]-like Attributen oder so.

**Well-known Attributes in .NET:**

* `[Flags]` bei Enums (wie oben genutzt),
* `[Serializable]` markiert serielle Klassen (BinaryFormatter etc.),
* `[DllImport]` und zugehörige für Interop,
* `[MarshalAs]` um zu definieren, wie Typ marshallt wird (unmanaged Signatur),
* `[CallerMemberName]`, `[CallerLineNumber]`, `[CallerFilePath]` – spezielle Attribute, die man an optionalen Parametern verwenden kann, damit der Compiler automatisch den Aufrufer-Namen/Zeilennr/Datei einfüllt (nützlich für Logging/INotifyPropertyChanged),
* `[Conditional("DEBUG")]` auf Methoden – bedeutet, diese Methode wird vom Compiler nur eingebunden, wenn der Symbol DEBUG definiert ist (z.B. `Debug.Assert` hat so was),
* `[STAThread]` auf Main in WinForms/WPF Apps – ruft COM Single-Thread Apartment initialisierung auf,
* etc.

**Pseudo-Attribute (assembly level):** Man kann Attribute auch auf Assembly-, Modul-, oder Parameterebene anwenden. Z.B. ganz oben in eine Datei:

```csharp
[assembly: AssemblyVersion("1.0.0.0")]
```

Das steuert die Assembly-Versionsnummer in Meta. Oder:

```csharp
[module: UnverifiableCode]
```

um anzugeben, dass der Modul unsicheren Code enthält. Parameter:

```csharp
void Foo([NotNull] string s) {...}
```

um an Parameter was zu definieren (z.B. in Code Contracts oder JetBrains Attributes, um static Analyzers mitzuteilen, dass s nicht null sein darf). Return:

```csharp
[return: MaybeNull]
public string? GetName(...) {...}
```

um am Rückgabewert Attribut anzugeben.

**Attribute und Reflection**: Um Attribute zu lesen, benutzt man Reflection-Methoden `MemberInfo.GetCustomAttributes` etc. Es gibt auch generische Variation `GetCustomAttribute<T>`. Für Performance: Attribut-Abfragen sind meist meta, aber man kann caches anlegen wenn oft genutzt.

In .NET Core sind manche Attribute in BCL aufgenommen für Nullability (z.B. `[NotNullWhen(false)]` auf Parameter, wie in \[27†L267-L275], die der Compiler in NRT-Analyse nutzt, um dem Code anzugeben "wenn diese Methode false zurückgibt, ist Parameter X not-null" etc.). Auch `[MaybeNull]`, `[DisallowNull]`, `[NotNull]` etc.

**Fazit:** Attribute erlauben decoupling von Metadaten und Logik – z.B. Markieren einer Methode als Test, ohne dass die Methode von Testing-Framework-Klassen erben oder Interfaces implementieren muss. Das Testing-Framework erkennt es rein per Attribut.

## Namespaces und Using-Direktiven

**Namespaces** helfen, den Code in Namensräume zu organisieren und Namenskollisionen zu vermeiden. Sie entsprechen weitgehend dem Konzept von Paketen/Modulen in anderen Sprachen. Ein Namespace wird deklariert mit:

```csharp
namespace MyCompany.MyProject.Utilities 
{
    public class Helper { ... }
    // ... weitere Typen
}
```

Alles innerhalb der `{ }` gehört nun zum Namespace "MyCompany.MyProject.Utilities". Der vollqualifizierte Name der Klasse Helper ist damit `MyCompany.MyProject.Utilities.Helper`. Namespaces können verschachtelt deklariert werden – entweder wie oben durch `.` in einem, oder auch:

```csharp
namespace MyCompany {
  namespace MyProject {
    class X { }
  }
}
```

Das ist aber selten, meist nutzt man die Punkt-Notation in einer Zeile.

Seit **C# 10** kann man auch *file-scoped* Namespaces verwenden, indem man das `{ }` weglässt und stattdessen ein `;` schreibt:

```csharp
namespace MyCompany.MyProject.Utilities;
public class Helper { ... }
```

Das gilt dann für die ganze Datei als Namespace (spart eine Indent-Stufe).

**using-Direktiven:** Um in einer Datei nicht immer vollqualifizierte Namen schreiben zu müssen, nutzt man `using`. Am Anfang einer .cs-Datei schreibt man z.B.:

```csharp
using System.Text;
using MyCompany.MyProject.Utilities;
```

Damit kann man direkt `StringBuilder sb = new StringBuilder();` schreiben, statt `System.Text.StringBuilder`. Der Compiler fügt beim Auflösen von `StringBuilder` automatisch System.Text als Suchraum hinzu. Using funktioniert für Namespaces *und* für static Klassen. Mit `using static SomeClass;` kann man statische Member einer Klasse direkt nutzen, z.B.:

```csharp
using static System.Math;
double r = Sqrt(Pow(x,2) + Pow(y,2));
```

Hier hat static using erlaubt, `Sqrt` und `Pow` ohne `Math.` zu schreiben.

Man kann auch **Alias-Using** machen:

```csharp
using Project = MyCompany.MyProject.Utilities;
...
Project.Helper.DoSomething();
```

Hier `Project` dient als Alias für den Namespace (oder Klasse) `MyCompany.MyProject.Utilities`. Das hilft etwa bei Namenskonflikten, oder um langen Namespacenamen abzukürzen lokal.

**Namensauflösung:** Der Compiler sucht in folgendem:

1. Aktueller Namespace (und umgebende Namespaces),
2. In allen `using`-Namespaces (und weiteren, falls `using alias =` etc.),
3. In `global::` falls prefix genutzt (erzwingt global namespace).
   Wenn zwei using Namespaces den gleichen Typ enthalten, muss man vollqualifizieren oder using Alias nutzen, sonst gibt's Compiler Error "Ambiguous reference".

Man kann im Code immer auch komplett qualifizierte Namen verwenden, beginnend mit `global::` wenn man den absoluten Pfad will (um etwa gleichnamigen Namespace in current overshadowing zu entgehen). `global::System.String` referenziert das BCL System.

**`namespace` vs `class` Accessibility:** Klassen können internal sein etc. Namespaces sind immer "öffentlich" im Sinne, was drin ist, kann per using im ganzen Projekt gesehen werden (internal-Klassen sind aber trotzdem assembly-intern). Namespaces existieren nur zur Compilezeit – in IL gibt es sie als Namensprefix.

**Namenskonventionen:** i.d.R. Unternehmen/Produkt modulare. BCL nutzt z.B. System.Collections.Generic.

**`using` als Anweisung:** (nicht zu verwechseln) – das `using` Statement wurde oben behandelt (für IDisposable). Hier geht es um using Directive.

**Globale Usings:** Neu in C# 10: man kann `global using NamespaceName;` schreiben, dann gilt das using für alle Dateien im Projekt – praktisch in .NET 6 Templates. Z.B. `global using System;` in einer Datei (die compiled wird, Reihenfolge egal?) erspart, dass in jeder Datei `using System` steht. .NET 6 hat sog. *implicit global usings* in SDK Projects, d.h. für gängige Namespaces braucht man nicht mal schreiben, es wird vom SDK generiert. Z.B. `System, System.Linq, System.Collections.Generic` etc. sind automatisch da in .NET6 `SDK <ImplicitUsings>` on.

**Verschiedenes:**

* Namespaces können über Assemblies hinweg weitergeführt werden. Z.B. `System.Xml` Namespace-Klassen sind in mehreren Assemblies verteilt.
* Eine Datei kann mehrere Namespace-Blöcke enthalten (aber selten sinnvoll, außer vielleicht gemischte code).
* Der *global Namespace* ist, was nicht in einem namespace-Block steht. Das sollte man vermeiden – lieber *alle* Code in Namespaces einschließen. Der global:: -Alias referenziert diesen absoluten Namensraum (Root).
* Namespaces haben keine Zugriffseinschränkungen (kein private/protected etc.), da sie eher logische Ordner sind.

## Präprozessor-Direktiven

C# bietet einige *Präprozessor-Direktiven*, die mit `#` beginnen. Anders als in C/C++ gibt es keinen separaten Präprozessor, aber der Compiler behandelt diese Anweisungen vor dem eigentlichen Kompilieren teilweise. Hier die wichtigsten:

* **`#define` und `#undef`:** Damit können *Kompiliersymbole* definiert oder entfernt werden. Diese Symbole sind einfache Namen (keine Werte) und werden v.a. für `#if`-Konfiguration genutzt. Beispiel am Dateianfang:

  ```csharp
  #define DEBUG
  #define TRIAL
  ```

  oder in Projektdatei über Build-Konfiguration. `#undef` entfernt ein Symbol. Diese Symbole sind **nicht** Variablen im Code (man kann ihren Wert nicht mit normalem C#-Code abfragen), sondern nur für die nachfolgenden `#if` etc. relevant.

* **`#if`, `#elif`, `#else`, `#endif`:** Bedingte Kompilierung. Sie ermöglichen, Code-Blöcke ein- oder auszublenden basierend auf definierten Symbolen. Beispiel:

  ```csharp
  #if DEBUG && !TRIAL
      Console.WriteLine("Debug build");
  #elif DEBUG && TRIAL
      Console.WriteLine("Debug Trial build");
  #else
      Console.WriteLine("Release build");
  #endif
  ```

  Hier werden zur Compilezeit je nach Symbolen unterschiedliche Codepfade eincompiliert. Alles, was in einer nicht erfüllten Sektion steht, wird vom Compiler ignoriert (als ob es auskommentiert wäre). Diese Konfigurationen sind nützlich z.B. um Debug-Logging nur in Debug-Builds zu haben, oder plattformspezifischen Code (Symbole wie `NET6_0_OR_GREATER`, `WINDOWS`, `ANDROID` etc. werden vom SDK gesetzt). Es gibt ein paar Standard-Symbole: `DEBUG` wird bei Debug-Builds automatisch definiert (je nach Projekteinstellung), `TRACE` oft auch, `NET5_0`, `NETSTANDARD2_0` etc. in neuen SDKs.

  Mehrfachverschachtelung ist erlaubt, aber man darf #if/endif nicht überlappen mit #region (müssen sauber geschachtelt sein). `#elif` ist "else if" für compile time, um mehrere Bedingungen in Kaskade zu prüfen. `#else` wenn alle obigen false.

* **`#warning` und `#error`:** Diese erzeugen beim Kompilieren eine Warn- bzw. Fehlermeldung mit einer angegebenen Textnachricht. Beispiel:

  ```csharp
  #if !DEBUG
  #warning "DEBUG not defined, assertions disabled!" 
  #endif
  ```

  oder

  ```csharp
  #if NET6_0_OR_GREATER
  #error "This code is for .NET Framework only!"
  #endif
  ```

  `#error` bricht die Kompilierung ab mit dem gegebenen Fehlertext (CS1029). `#warning` gibt eine Warnung (CS1030) aus, die den Build nicht stoppt.

* **`#region` und `#endregion`:** Diese definieren frei benennbare Codeblöcke, die im Editor ein- und ausklappbar sind. Sie haben keine Semantik für den Compiler (der ignoriert diese Blöcke, außer dass #region/#endregion korrekt paarig sein müssen). Beispiel:

  ```csharp
  #region Datenfelder
  private int count;
  private string name;
  #endregion
  ```

  In Visual Studio kann man so den Code übersichtlich gliedern. Overlapping von Regionen mit ifs ist nicht erlaubt, aber verschachteln geht.

* **`#pragma` Direktiven:**

  * `#pragma warning disable CS0168` z.B. kann bestimmte Compilerwarnungen unterdrücken (hier: ungenutzte Variable) ab dem Punkt; man kann mit `#pragma warning restore CS0168` wieder aktivieren. Man kann auch `disable 0168` ohne "CS" schreiben.
  * `#pragma checksum` und `#pragma default`, `#pragma warning` etc. gibts.
  * `#pragma warning disable` ohne Codes gilt global alle Warnungen aus, was man selten tun sollte.
  * `#pragma` wird oft benutzt in generiertem Code, um irrelevante Warnungen lokal abzuschalten.

* **`#line` Directive:** Ermöglicht es, die vom Compiler ausgegebene Zeilennummer und Datei für den folgenden Code zu verändern. Syntax: `#line 100 "SomeFile.cs"`. Das wird genutzt bei Quellcodegeneratoren, um korrekte Fehlerpositionsmeldungen auf das Ursprungsfile zu mapen. Oder `#line hidden` um einen Block vom Debugger zu verstecken. Normalerweise braucht man das nicht manuell.

* **`#nullable` Directive:** Ab C# 8 kann man innerhalb einer Datei die *Nullable-Context* toggeln. `#nullable enable` schaltet die Nullability-Anmerkungen und -Warnungen ein (so als ob in Projekt aktiviert). `#nullable disable` schaltet es aus. Es gibt auch getrennt `#nullable enable warnings` oder `annotations`. Usually `#nullable enable` at top of old code files to gradually opt-in. In neuen Projekten global mit <Nullable>enable</Nullable> im csproj. Mit `#nullable restore` kann man auf Projektdefault zurücksetzen im Code.

* **`#if DEBUG` vs `ConditionalAttribute`:** Neben der Präprozessorweise gibt es auch Attribut `[Conditional("DEBUG")]` auf Methoden. Das führt dazu, dass Aufrufe dieser Methode vom Compiler nur in einem Build kompiliert werden, wenn das Symbol definiert ist. Beispiel:

  ```csharp
  [Conditional("DEBUG")]
  void Log(string msg) { Console.WriteLine(msg); }
  ...
  Log("Only in debug");
  ```

  Im Release-Build wird der Call komplett entfernt. Der Vorteil von Conditional-Attr: Man muss nicht mit #if den Aufruf umschließen – klarer. Der Nachteil: Geht nur auf Methoden mit void return (kein wirklicher Nachteil, weil sinnvolle Cases sind Logging, Assert etc.). Das wird z.B. bei Debug.Assert so gemacht: `[Conditional("DEBUG")]` im BCL.

**Wichtig:** Präprozessor-Direktiven wirken dateiweit, es gibt keine globale Symboltabelle pro Project (außer was in csproj definert). Ein `#define X` am Anfang einer Datei gilt *nur in dieser Datei*. Will man ein Symbol projektweit definieren, macht man das in den Projekteinstellungen (MSBuild: `<DefineConstants>DEBUG;TRACE;SOMETHING</DefineConstants>`). Oder man wiederholt #define in jeder betroffenen Datei (unschön).

**Kommentare:** `//` und `/* ... */` sind keine Direktiven, aber der Vollständigkeit halber: `//` für Einzelzeile, `/* */` für Blockkommentar (nicht verschachtelbar in C#; Trick: `/**/` leerer block, anyway).

## Unsicherer Code (Unsafe Code)

C# erlaubt mittels dem Modifier **`unsafe`** die Verwendung von Pointer-Arithmetik und direktem Speicherzugriff – ähnlich wie in C/C++. Unsicherer Code wird als nicht-verifizierbar betrachtet (kann Sicherheitsverletzungen verursachen) und benötigt in den Projekt-Einstellungen die Option "Allow unsafe" (bzw. Compilerflag `/unsafe`). Unsafe heißt nicht, dass es gar nicht verwaltet ist – man kann es im managed Programm nutzen, aber man verlässt damit die Garantien der CLR (z.B. Type Safety, Memory Safety).

**Pointer-Syntax:** In unsafe-Kontext kann man Zeigertypen deklarieren: `int* p;` ist ein Pointer auf int. Er kann auf eine native Speicheradresse zeigen. Man kann den Adressoperator `&` benutzen, um Zeiger auf einen Wert zu erhalten, und den Dereferenz-Operator `*` um auf den Inhalt zuzugreifen. Z.B.:

```csharp
unsafe 
{
    int x = 42;
    int* p = &x;       // p enthält Adresse von x
    Console.WriteLine(*p);  // druckt 42
    *p = 100;               // verändert x direkt
}
```

Außerhalb von `unsafe`-markierten Methoden oder Blöcken sind solche Operationen nicht erlaubt.

**Pointertypen:** `T*` ist erlaubt für T = any unmanaged type (Werttypen oder auch struct mit nur unmanaged Feldern). Pointers auf Referenztypen (Klassen) sind vom CLR nicht erlaubt, aber man kann pointer auf generische T in unsafe definieren, solange T struct unmanaged ist. Es gibt spezielle Funktion-Zeiger seit C# 9 `delegate*` (dazu später).

**Stackalloc:** Man kann auf dem Stack *unverwalteten* Speicher reservieren mit `stackalloc`. Beispiel:

```csharp
unsafe 
{
    int* arr = stackalloc int[100];
    for(int i=0; i<100; i++)
       arr[i] = i;
}
```

`stackalloc` allokiert hier 100 ints auf dem Stack (innerhalb des aktuellen Method-Stackframes). Sobald der block verlassen wird (Ende unsafe block oder Ende Methode), ist der Speicher automatisch freigegeben (Stack entpuppt). Dies vermeidet GC und ist sehr schnell, aber man muss aufpassen, nicht mehr allozieren als der Stack verträgt (StackOverflow möglich bei sehr großen Arrays). Seit C# 7.2 kann man `stackalloc` auch *span-safe* nutzen: `Span<int> span = stackalloc int[100];` – dann kann man auch in safe code damit arbeiten, dank `Span<T>` (der Span struct hat einen ref pointer intern). So kann man temporären Buffer allozieren ohne GC.

**Fixed Statement:** Um Zeiger auf managed Objekte zu bekommen (z.B. auf ein Arrayelement, string chars, etc.), muss man das Objekt *pin*nen mit `fixed`-Statement. Beispiel:

```csharp
unsafe 
{
    byte[] data = new byte[10];
    fixed(byte* p = data) 
    {
        // jetzt ist data während des Blocks gepinnt im Heap, GC verschiebt es nicht
        p[0] = 123;
    }
    // Nach dem Block darf p nicht mehr verwendet werden, und data kann wieder bewegt werden.
}
```

Ohne `fixed` würde `byte* p = data` nicht kompilieren (cannot convert array to pointer). Das `fixed`-Statement kann auch Strings pinnen: `fixed(char* p = someString)` – dann ist p auf die UTF-16-Daten (Achtung: Strings sind immutabel, nicht verändern!). Oder auf einen `fixed`-Buffer in einem struct (C# erlaubt in unsafe struct sog. fixed buffer fields, wie in \[64†L265-L274] und \[64†L279-L287] gezeigt, um z.B. `public fixed char Name[30];` innerhalb eines struct definieren zu können – das erzeugt ein inline-Array als field).

**Pointer-Arithmetik:** Auf Zeiger kann man rechnen: `p + 1` bewegt den Zeiger um die Größe des Typs weiter. Also `int* p; p++` erhöht Adresse um 4 Bytes. Man kann auch Vergleichsoperatoren auf Zeiger anwenden (<, >, etc.). Aber man sollte sicher sein was man tut – off-by-one kann Crash (oder Memory Corruption) bedeuten.

**Dereferenzierung:** `*p` greift auf den Wert an der Adresse. Das kann unsichere Operation sein: Wenn p ungültig ist (wild pointer), stürzt Programm i.d.R. ab oder schlimmer: potentielle Ausnutzung. Der CLR JIT macht keine Bounds-Checks bei pointer, das ist die Verantwortung des Programmierers.

**Beispiel** (Kopieren eines Array-Segments mit unsafen Code):

```csharp
unsafe 
{
    int[] source = {1,2,3,4,5};
    int[] dest = new int[source.Length];
    fixed(int* pSrc = source, pDest = dest) 
    {
        // Kopiert via Pointer:
        for(int i=0; i< source.Length; i++) 
        {
            pDest[i] = pSrc[i];
        }
    }
}
```

Das würde natürlich auch mit Array.Copy schneller gehen, aber zeigt pointer use.

**Function Pointers:** Ab C# 9 gibt es `delegate*` Syntax in unsafe context, um echte unverankerte Funktionszeiger (z.B. zu native Funktionen oder static managed functions) zu nutzen. Syntax z.B.: `delegate* unmanaged[Cdecl]<int,int,int> funcPtr = ...;`. Das legt einen Zeiger auf eine C-Funktion fest, die int,int->int hat. Dann kann man `int result = funcPtr(5,10);` aufrufen. Für Platform Invoke hat man bisher DllImport, aber function pointer sind low-level Performance Trick (z.B. Interop mit dx). Eher fortgeschritten.

**Interoperabilität:** Oft wird unsafe benötigt, um mit unmanaged Code zu reden, z.B. Strukturen blockkopieren, Bytes zu Int casten, Arbeit mit memory-mapped devices, etc.

**`stackalloc` in safe code via Span:** Um Buffer ohne GC im safe code zu erzeugen, nutzt man `Span<T>`:

```csharp
Span<byte> buffer = stackalloc byte[256];
```

Kein `unsafe` nötig. Span ist ref struct, der 'point' intern auf den stack buffer. Das ist high-level safe Alternative.

**Performance:** Unsicherer Code kann Performancegewinne bringen, aber .NET JIT hat sich verbessert – in manchen Fällen bringt Span oder Array-Methoden gleiches. Use-case: extrem hohe Performance loops, implementing algorithms like memory copy, image processing inner loops etc. Aber C#-Compiler gab uns `System.Runtime.CompilerServices.Unsafe` class mit generischen Hilfsfunktionen, um safe code tricky pointer zu machen (auch advanced)...

**Sicherheitsaspekt:** Um unsafe Code auszuführen, braucht die Anwendung je nach Umgebung passende Security (im .NET Core Modell uninteressant, im .NET Framework hatte es CAS (Code Access Security) for skip verification). In normal Desktop .NET, if user runs exe, unsafe is fine. In partially trusted environment (like older plugin scenario) war es restricted.

**Zusammenfassung:** *Unsafe* Code gibt C# Entwickler die Möglichkeit, low-level Memory-Manipulation zu machen, wenn nötig. Es sollte sparsam und mit Know-How eingesetzt werden, da es leicht zu Abstürzen oder schwer auffindbaren Memory Corruptions führen kann. Für viele Aufgaben gibt es sichere Alternativen (Span, Marshal class im Interop). Aber z.B. high-performance serializations, working with big byte arrays etc. oft noch pointer.

Im .NET Umfeld wird unsafe oft in Interop-Layern, game development (MonoGame, Unity's Burst etc.), performance-critical libraries (e.g. Span-based ones) genutzt. Normal line-of-business dev kommt selten damit in Berührung.

## XML-Dokumentation

C# bietet die **XML-Dokumentationskommentare**, um den Code mit strukturierten Beschreibungen zu versehen, die von Tools (IntelliSense, Dokumentationsgeneratoren) genutzt werden können. Wenn man `///` oberhalb einer Definition schreibt, beginnt ein XML-Doku-Kommentar. Beispiel:

```csharp
/// <summary>Berechnet die Summe zweier Zahlen.</summary>
/// <param name="a">Der erste Summand.</param>
/// <param name="b">Der zweite Summand.</param>
/// <returns>Die Summe von a und b.</returns>
public int Add(int a, int b) => a + b;
```

Hier wurden die standardmäßigen Tags verwendet:

* `<summary>`: Ein kurzer Beschreibungstext des Typs oder Members. Wird z.B. in IntelliSense angezeigt.
* `<param name="...">`: Beschreibt einen Parameter (jeweils mit dem entsprechenden Namen).
* `<returns>`: Beschreibung des Rückgabewerts (bei void weglassen).

Weitere wichtige Tags sind:

* `<remarks>`: Für ausführlichere Erläuterungen, Ergänzungen oder Beispiele. Wird oft genutzt, um einen längeren Abschnitt zu schreiben, der über die Summary hinausgeht (z.B. Einschränkungen, Details).
* `<example>`: Man kann innerhalb von remarks ein `<example>`-Tag nutzen oder separat. Einige Doku-Generatoren zeigen Beispiele extra an. Inhalt könnte ein `<code>`-Block sein, z.B. `<code lang="csharp"> ... </code>` mit Codebeispiel.
* `<exception cref="ExceptionType">`: Gibt an, dass die Methode diese Exception auslösen kann (z.B. `<exception cref="ArgumentNullException">Wenn input null ist.</exception>`).
* `<seealso>`: Querverweis auf andere Klasse/Member-Doku. Bsp: `<seealso cref="OtherClass"/>` führt in generierter Hilfe zu Link auf OtherClass. `<see>` (Inline) kann im Fließtext verwendet werden, um Referenzen zu code einzubauen. Z.B. `<see cref="System.String"/>` in Text, Format: *String* (hyperlink).
* `<value>`: Speziell für Properties, um den get/set-Wert zu beschreiben (oft redundant mit summary, aber es wird empfohlen, Summary für Typ/Property an sich, Value-Tag für was Property gibt).
* `<typeparam name="T">`: Für generische Typparameter Erklärung.
* `<para>`: Kann in remarks etc. benutzt werden, um Absatz zu machen (in generierter Doku neuer Absatz).
* `<c>`: Für Inline-Code (monospace) in Text, `<code>`: für block code (formatierter Pre-Bereich).
* `<list>`: Um Aufzählungen/Tabelle in Beschreibung zu formatieren (selten manuell).
* `<inheritdoc>`: (C# >= 9) Kann genutzt werden, um die Dokumentation vom Basiselement zu erben (erspart copy-paste, Tools wie DocFX unterstützen dies, es ist recommended tag).

Man kann eigene XML-Tags definieren, aber der Compiler meckert nur, wenn das Tag unbekannt ist (er warnt bei Tippfehlern, Standardtags sind dem Compiler bekannt). Standardtags wie summary, param, returns etc. werden von IntelliSense genutzt.

**Dokugenerierung:** Mit dem Compiler-Flag `/doc` kann man eine XML-Datei erzeugen lassen, die alle Kommentare gesammelt enthält. In Projektdatei oft `<GenerateDocumentationFile>true</GenerateDocumentationFile>`. Diese XML kann dann von Tools (z.B. Sandcastle, DocFX, VS Intellisense) gelesen werden. Visual Studio IntelliSense zeigt summary, param, returns etc. an, wenn man drüber hovert oder Parameter-Info nutzt.

**Beispiel:**

```csharp
/// <summary>
/// Repräsentiert einen 2D-Punkt.
/// </summary>
/// <remarks>
/// Diese Struktur ist unveränderlich. Sie bietet Methoden zur Distanzberechnung.
/// </remarks>
public struct Point
{
    /// <summary>X-Koordinate.</summary>
    public int X { get; }
    /// <summary>Y-Koordinate.</summary>
    public int Y { get; }

    /// <summary>Erzeugt einen Punkt mit gegebenen Koordinaten.</summary>
    /// <param name="x">Die X-Koordinate.</param>
    /// <param name="y">Die Y-Koordinate.</param>
    public Point(int x, int y) { X = x; Y = y; }

    /// <summary>Berechnet die Entfernung zu einem anderen Punkt.</summary>
    /// <param name="other">Der andere Punkt.</param>
    /// <returns>Die Distanz zwischen den Punkten.</returns>
    /// <example>
    /// <code>
    /// var p1 = new Point(0,0);
    /// var p2 = new Point(3,4);
    /// double dist = p1.DistanceTo(p2);
    /// // dist = 5
    /// </code>
    /// </example>
    public double DistanceTo(Point other) => Math.Sqrt(Math.Pow(other.X - X, 2) + Math.Pow(other.Y - Y, 2));
}
```

So eine Dokumentation ermöglicht, dass in IntelliSense z.B. bei `DistanceTo` die Beschreibung erscheint und Parameter-Name Doku.

**Best Practices:**

* Halte `<summary>` knapp (eine Zeile bis ein paar), da sie primär in Intellisense Pop-up erscheint.
* Endet `<summary>` traditionell mit Punkt.
* Documentiere alle öffentlichen APIs; optional interne wenn interne Doku generiert wird.
* `<remarks>` für ausführlichere Ausführungen, oft mit Format.
* Konsistenz: z.B. "Gets or sets ..." Patterns bei Properties.
* Tools können Warnungen ausgeben, wenn öffentlicher Member kein Doku hat (CS1591), je nach Projekt-Einstellung.

**In Code**: man nutzt 3x `/` um Dokumentationskommentar zu beginnen; VS generiert Gerüst, wenn man `///` vor der Methode eingibt.

**Verhältnis zum normale Kommentare:** Normale `//` oder `/* */` sind nur für Entwickler im Quelltext. XML-Doku ist für Endnutzer der API und taucht in IntelliSense/Hilfedateien auf.

**Nicht alle Entities können doc haben:** Namespaces selber nicht dokumentierbar per comment (kann assembly-level aushelfen), aber Klassen, Methoden, Properties etc. ja.

Das deckt den Großteil der Sprache ab. Diesem Cheat Sheet kann man entnehmen, dass C# eine umfangreiche, moderne Sprache mit vielen Features ist – hier in komprimierter Form zusammengefasst.
