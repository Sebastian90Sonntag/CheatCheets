# 📘 TypeScript & JavaScript Cheat Sheet

Ein kompakter Überblick über Typisierung, Interfaces, Klassen, Module, Utility Types u.v.m. für TypeScript und JavaScript.

---

## 🟨 JavaScript Essentials

### Variablen

```js
const pi = 3.14;
let counter = 0;
var legacy = true; // vermeide var!
```

### Funktionen

```js
function greet(name) {
  return `Hello, ${name}`;
}

const sum = (a, b) => a + b;
```

### Objekte & Arrays

```js
const user = {
  name: 'Alice',
  age: 30
};

const tags = ['js', 'cheatsheet'];
```

### Klassen (ab ES6)

```js
class Car {
  constructor(brand) {
    this.brand = brand;
  }
  drive() {
    console.log(`${this.brand} is driving`);
  }
}
```

### Module (ab ES6)

```js
// export.js
export const PI = 3.1415;

// import.js
import { PI } from './export.js';
```

### Promises & Async/Await

```js
async function fetchData(url) {
  const res = await fetch(url);
  const data = await res.json();
  return data;
}
```

### Spread, Destructuring, Optional Chaining

```js
const obj = { a: 1, b: 2 };
const clone = { ...obj };

const { a, b } = obj;

console.log(obj?.a);
```

### Array-Funktionen

```js
[1, 2, 3].map(x => x * 2);
[1, 2, 3].filter(x => x > 1);
[1, 2, 3].reduce((a, b) => a + b);
```

### JSON

```js
const str = JSON.stringify(user);
const parsed = JSON.parse(str);
```

---

## ⚠️ JavaScript Besonderheiten & Fallstricke

* `==` vs `===`: Immer `===` verwenden (Typprüfung!)
* `null` und `undefined` sind unterschiedlich
* `NaN !== NaN` → nutze `Number.isNaN()`
* `typeof null === 'object'` → historischer Bug
* `this` ist kontextabhängig → arrow functions bevorzugen
* Primitive vs Referenztypen: Änderungen an Objekten wirken sich auf alle Referenzen aus

---

## 💡 Tipps & Tricks für JavaScript

* Nutze `??` (Nullish Coalescing) statt `||`, um falsy Werte korrekt zu behandeln

  ```js
  const name = input ?? 'Standard';
  ```
* Verwende `Object.freeze(obj)` für konstante Objekte
* Nutze `Object.entries()` & `Object.fromEntries()` für Mapping
* Verwende `Intl.DateTimeFormat` & `Intl.NumberFormat` für Lokalisierung
* Codiere defensiv mit optional chaining `?.` und default-Werten `??`

---

## 🔐 Sicherheitstipps für JavaScript

* Validierung von Benutzereingaben IMMER client- UND serverseitig
* Niemals `eval()` verwenden → Sicherheitsrisiko
* Keine sensitiven Daten im Frontend speichern (z.B. API-Schlüssel)
* HTTP-Anfragen mit CSRF-Token und CORS absichern
* Escape von HTML-Ausgabe zur Vermeidung von XSS:

  ```js
  element.textContent = userInput;
  ```
* Nutze sichere Bibliotheken: z. B. DOMPurify zur Bereinigung von HTML
* Arbeite mit Content Security Policy (CSP) auf Serverseite

---

(Die bestehenden TypeScript-Inhalte bleiben wie im aktuellen Dokument bestehen)

---

## 📚 Zusätzliche Ressourcen (JS)

* [developer.mozilla.org](https://developer.mozilla.org/de/docs/Web/JavaScript)
* [javascript.info](https://javascript.info)
* [ECMAScript Vorschläge](https://github.com/tc39/proposals)
* [Frontend Master’s JS Path](https://frontendmasters.com/learn/javascript/)
* [DOMPurify – XSS Schutz](https://github.com/cure53/DOMPurify)
* [OWASP XSS Guide](https://owasp.org/www-community/attacks/xss/)

(Diese JS-Sektion ist speziell für Entwickler, die sowohl mit TypeScript als auch mit Vanilla JavaScript arbeiten.)
