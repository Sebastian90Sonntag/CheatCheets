# 📘 TypeScript Cheat Sheet

Ein kompakter Überblick über Typisierung, Interfaces, Klassen, Module, Utility Types u.v.m.

---

## ⚙️ Grundlagen

```ts
// explizite Typen
let age: number = 25;
let username: string = 'Max';
let isAdmin: boolean = true;

// Arrays & Tupel
let tags: string[] = ['typescript', 'cheatsheet'];
let point: [number, number] = [10, 20];

// Union & Literal Types
let id: number | string = 123;
let direction: 'left' | 'right' = 'left';
```

---

## 📐 Funktionen

```ts
function add(a: number, b: number): number {
  return a + b;
}

const log = (msg: string): void => {
  console.log(msg);
}

// optionale Parameter & Defaults
function greet(name: string = 'User') {}
```

---

## 🧩 Interfaces & Typen

```ts
interface User {
  id: number;
  name: string;
  isActive?: boolean;
}

// Typalias
type Point = {
  x: number;
  y: number;
}

function printUser(u: User | Point) {}
```

---

## 🧱 Klassen

```ts
class Person {
  constructor(public name: string, private age: number) {}

  greet(): void {
    console.log(`Hi, I'm ${this.name}`);
  }
}
```

### 🧪 Komplette Beispielklasse mit Gettern, Settern, Static & Inheritance

```ts
class Employee {
  private _salary: number;
  static company = 'Acme Corp';

  constructor(public readonly id: number, public name: string, salary: number) {
    this._salary = salary;
  }

  get salary(): number {
    return this._salary;
  }

  set salary(amount: number) {
    if (amount < 0) throw new Error('Invalid salary');
    this._salary = amount;
  }

  describe(): string {
    return `${this.name} earns ${this._salary} at ${Employee.company}`;
  }
}

class Manager extends Employee {
  constructor(id: number, name: string, salary: number, public department: string) {
    super(id, name, salary);
  }

  override describe(): string {
    return super.describe() + ` and manages ${this.department}`;
  }
}
```

---

## 🧰 Utility Types

```ts
Partial<User>       // macht alle Felder optional
Required<User>      // macht alle Felder Pflicht
Pick<User, 'id'>    // nur id
Omit<User, 'id'>    // alle außer id
Record<string, any> // Key-Value Map
```

---

## 🔁 Generics

```ts
function identity<T>(value: T): T {
  return value;
}

interface ApiResponse<T> {
  data: T;
  error?: string;
}
```

---

## 🔐 Enums

```ts
enum Role {
  Admin = 'ADMIN',
  User = 'USER'
}

let userRole: Role = Role.User;
```

---

## 🧱 Module & Namespaces

```ts
// Datei: math.ts
export function add(a: number, b: number) {
  return a + b;
}

// Datei: app.ts
import { add } from './math';
```

---

## 🧪 Type Guards & Assertion

```ts
function isString(val: any): val is string {
  return typeof val === 'string';
}

let input = someVal as string;
```

---

## 🧠 Advanced

```ts
// Mapped Types
 type ReadonlyUser = {
   readonly [K in keyof User]: User[K];
 }

// Conditional Types
 type Message<T> = T extends string ? string : never;
```

---

## 💡 Besonderheiten & Tipps

* `readonly` schützt Felder vor späterer Veränderung
* Mit `as const` lassen sich Literale fixieren (`const roles = ['admin', 'user'] as const;`)
* Union Discriminierung mit gemeinsamen `kind` Feldern für saubere Switch-Logik
* `never` zeigt an, dass ein Codepfad nicht erreichbar ist
* `unknown` ist sicherer als `any` und zwingt zur Typprüfung
* Vermeide `null` und arbeite mit `undefined` & optional chaining `?.`
* Verwende `strict`-Modus für sauberen Code und bessere IntelliSense-Unterstützung
* Deklariere `const enum` für Performance bei gleichzeitiger Typprüfung
* Nutze `infer` in bedingten Typen zur Typableitung
* Verwende `satisfies` zur Typprüfung bei Objektliteralen ohne Einschränkung auf exakt gleiche Struktur

```ts
const settings = {
  theme: 'dark',
  language: 'de'
} satisfies Record<string, string>;
```

---

## ⚛️ React + TypeScript

```tsx
interface ButtonProps {
  label: string;
  onClick: () => void;
}

const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);

const App: React.FC = () => {
  return <Button label="Click me" onClick={() => alert('Hi')} />;
};
```

**React-Tipps:**

* Props & State immer typisieren
* `useRef<HTMLInputElement>()` für DOM-Zugriffe
* `React.FC` ist optional, explizite Props-Definition bevorzugt

---

## 🚀 NestJS + TypeScript

```ts
// DTO (Data Transfer Object)
export class CreateUserDto {
  username: string;
  password: string;
}

// Controller
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }
}

// Service
@Injectable()
export class UsersService {
  create(dto: CreateUserDto) {
    return { user: dto, created: true };
  }
}
```

**NestJS-Tipps:**

* Verwende `@nestjs/swagger` für Doku
* DTOs mit `class-validator` validieren
* Typisierte Services und Module für Testbarkeit und Klarheit

---

## 🧩 Angular + TypeScript

```ts
// Component
@Component({
  selector: 'app-greeting',
  template: `<h1>Hello {{name}}</h1>`
})
export class GreetingComponent {
  name: string = 'Angular';
}

// Service
@Injectable({ providedIn: 'root' })
export class DataService {
  getData(): string[] {
    return ['Angular', 'Rocks'];
  }
}
```

**Angular-Tipps:**

* `strictTemplates` in tsconfig.json aktivieren
* Interfaces für Inputs und Outputs verwenden
* Services immer typisieren und Dependency Injection nutzen

---

## 🧷 Vue 3 + TypeScript (Composition API)

```ts
<script lang="ts" setup>
import { ref } from 'vue';

const count = ref<number>(0);
function increment(): void {
  count.value++;
}
</script>

<template>
  <button @click="increment">Clicked {{ count }} times</button>
</template>
```

**Vue-Tipps:**

* Immer `lang="ts"` und `<script setup>` verwenden
* Props & Emits über `defineProps` und `defineEmits` typisieren

---

## 🧪 Testing mit TypeScript

* **Jest**: Test-Runner für Unit & Integration
* **ts-jest**: TypeScript-Support für Jest

```ts
// Beispiel-Test
import { add } from './math';
test('add returns sum', () => {
  expect(add(1, 2)).toBe(3);
});
```

* Testdateien als `*.spec.ts` oder `*.test.ts`

---

## 🧵 Decorators mit TypeScript

```ts
function Log(target: any, key: string) {
  let value = target[key];
  const getter = () => {
    console.log(`Get: ${key} => ${value}`);
    return value;
  };
  const setter = (newVal: any) => {
    console.log(`Set: ${key} => ${newVal}`);
    value = newVal;
  };
  Object.defineProperty(target, key, { get: getter, set: setter });
}

class Example {
  @Log
  title: string = 'Hello';
}
```

---

## ⚙️ CI/CD mit TypeScript

* **Linting:** `eslint` mit `@typescript-eslint`
* **Build:** `tsc` oder `esbuild`
* **Testing:** Jest oder Vitest
* **Deployment:** GitHub Actions / GitLab CI / Azure DevOps

**Beispiel GitHub Action:**

```yaml
name: CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npm run lint
      - run: npm run test
      - run: npm run build
```

---

## 🔧 tsconfig.json (Beispiel)

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "ESNext",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

---

## 📚 Ressourcen

* [typescriptlang.org/docs](https://www.typescriptlang.org/docs/)
* [tsconfig.dev](https://tsconfig.dev)
* [type-challenges](https://github.com/type-challenges/type-challenges)
* [typescript-exercises](https://typescript-exercises.github.io/)
* [ts-pattern (Pattern Matching)](https://github.com/gvergnaud/ts-pattern)
* [Type-Level TypeScript](https://github.com/orta/TypeScript/wiki/TypeScript-Deep-Dive)
* [React + TS Docs](https://react-typescript-cheatsheet.netlify.app/)
* [NestJS Doku](https://docs.nestjs.com/)
* [Angular Doku](https://angular.io/docs)
* [Vue 3 + TS Guide](https://vuejs.org/guide/typescript/overview.html)
* [Jest (ts-jest)](https://kulshekhar.github.io/ts-jest/)
* [GitHub Actions Docs](https://docs.github.com/actions)
