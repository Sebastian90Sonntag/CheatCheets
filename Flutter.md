# 📱 Flutter Cheat Sheet

Ein kompakter Überblick über die wichtigsten Flutter-Konzepte, Widgets, Layouts und Befehle.

---

## 📦 Setup & CLI

```bash
flutter create my_app
cd my_app
flutter run
flutter build apk  # oder ios, web, windows
flutter doctor
flutter pub get / upgrade / outdated
flutter clean
```

---

## 🧱 Grundlegende Widgets

```dart
MaterialApp()       // Material Design App-Rahmen
Scaffold()          // Grundstruktur mit AppBar, Body, Drawer
AppBar(title: ...)  // Top-Navigation
Text('Hello')       // Textanzeige
Image.asset(...)    // Bild aus assets
Icon(Icons.add)     // Symbol anzeigen
ElevatedButton(...) // Erhöhter Button
TextButton(...)     // Flacher Button
```

---

## 🤭 Navigation & Routing

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondPage()),
);

Navigator.pop(context);  // zurück

// Named Routes
MaterialApp(
  routes: {
    '/': (context) => HomePage(),
    '/second': (context) => SecondPage(),
  },
);
Navigator.pushNamed(context, '/second');
```

---

## 🧹 Layout Widgets

```dart
Row(...)           // horizontal
Column(...)        // vertikal
Container(...)     // Box mit Styling
Padding(...)       // Abstand innen
Margin(...)        // über Container → `Container(margin: ...)`
Expanded(...)      // flexibler Bereich
Center(...)        // zentriert Kind
SizedBox(...)      // feste Größe
```

---

## 🎛 Formulare & Eingaben

```dart
TextField(
  controller: myController,
  decoration: InputDecoration(labelText: 'E-Mail'),
)

TextFormField(...)  // mit Validator

Form(
  key: _formKey,
  child: ...
)
_formKey.currentState.validate();
_formKey.currentState.save();
```

---

## 🎨 Styling & Themes

```dart
Text(
  'Text',
  style: TextStyle(
    color: Colors.blue,
    fontWeight: FontWeight.bold,
    fontSize: 18,
  ),
)

ThemeData(
  primarySwatch: Colors.blue,
  brightness: Brightness.dark,
)
```

---

## ⚛️ State Management (setState)

```dart
setState(() {
  counter++;
});
```

---

## ⚛️ State Management (Provider - Beispiel)

```dart
class Counter with ChangeNotifier {
  int _count = 0;
  int get count => _count;
  void increment() {
    _count++;
    notifyListeners();
  }
}

// Zugriff:
Provider.of<Counter>(context).increment();
```

---

## 🔗 HTTP Requests

```dart
import 'package:http/http.dart' as http;

final response = await http.get(Uri.parse('https://api.example.com/data'));
if (response.statusCode == 200) {
  final data = jsonDecode(response.body);
}
```

---

## 📂 Assets einbinden (pubspec.yaml)

```yaml
flutter:
  assets:
    - assets/images/logo.png
    - assets/data.json
```

---

## ✅ Linting & Tests

```bash
flutter analyze
flutter test
```

---

## 🦪 Beispiel: Widget-Test

```dart
testWidgets('MyWidget has a title', (WidgetTester tester) async {
  await tester.pumpWidget(MyWidget());
  expect(find.text('Title'), findsOneWidget);
});
```

---

## 📚 Weiterführende Ressourcen

* 📖 [official Flutter docs](https://flutter.dev/docs)
* 🧪 [pub.dev](https://pub.dev)
* 🎨 [Flutter Widget Catalog](https://flutter.dev/widgets)
