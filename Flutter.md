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

## 🔐 Peer-to-Peer (P2P) Kommunikation & Verschlüsselung

### Architektur für echtes P2P ohne Referenzserver

**Herausforderungen:**
- Geräte befinden sich meist hinter NAT/Firewall
- Direkte Verbindung erfordert NAT-Traversierung
- End-to-End-Verschlüsselung zusätzlich zur Transportverschlüsselung

**Lösungsansätze:**

#### 1. **WebRTC (empfohlen für Internet P2P)**

```dart
// Package: flutter_webrtc
dependencies:
  flutter_webrtc: ^0.9.0

// Minimaler WebRTC-Setup
import 'package:flutter_webrtc/flutter_webrtc.dart';

// 1. Peer Connection erstellen
final config = {
  'iceServers': [
    {'urls': 'stun:stun.l.google.com:19302'}  // Nur für NAT-Traversierung
  ]
};
RTCPeerConnection pc = await createPeerConnection(config);

// 2. Data Channel für P2P-Daten
RTCDataChannel dataChannel = await pc.createDataChannel('myChannel', 
  RTCDataChannelInit()..ordered = true);

// 3. Daten senden (verschlüsselt über DTLS)
dataChannel.send(RTCDataChannelMessage('Hallo P2P!'));

// 4. Daten empfangen
dataChannel.onMessage = (RTCDataChannelMessage message) {
  print('Empfangen: ${message.text}');
};
```

**Signaling (nur für initialen Verbindungsaufbau):**
- Einmalig SDP-Offer/Answer austauschen (z.B. via QR-Code, Bluetooth, manuell)
- Danach: direkte P2P-Verbindung ohne Server

#### 2. **Nearby Connections (lokal, ohne Internet)**

```dart
// Package: nearby_connections
dependencies:
  nearby_connections: ^3.0.0

import 'package:nearby_connections/nearby_connections.dart';

// Advertiser (Empfänger)
Nearby().startAdvertising(
  userName: 'Device1',
  strategy: Strategy.P2P_CLUSTER,
  onConnectionInitiated: (String id, ConnectionInfo info) {
    Nearby().acceptConnection(id, onPayLoadRecvCallback: (endpointId, payload) {
      String message = String.fromCharCodes(payload.bytes!);
      print('Empfangen: $message');
    });
  },
);

// Discoverer (Sender)
Nearby().startDiscovery(
  userName: 'Device2',
  strategy: Strategy.P2P_CLUSTER,
  onEndpointFound: (String id, String name, String serviceId) {
    Nearby().requestConnection(userName: 'Device2', id: id,
      onConnectionResult: (id, status) {
        if (status == Status.CONNECTED) {
          // Verbindung erfolgreich
          Nearby().sendBytesPayload(id, Uint8List.fromList('Hallo'.codeUnits));
        }
      }
    );
  },
);
```

#### 3. **Zusätzliche End-to-End-Verschlüsselung**

WebRTC nutzt bereits DTLS/SRTP, aber für zusätzliche E2E-Verschlüsselung:

```dart
// Package: pointycastle oder cryptography
dependencies:
  cryptography: ^2.7.0

import 'package:cryptography/cryptography.dart';

// AES-GCM Verschlüsselung
final algorithm = AesGcm.with256bits();

// Schlüsselaustausch (z.B. via Diffie-Hellman)
final keyPair = await algorithm.newKeyPair();

// Verschlüsseln
Future<List<int>> encrypt(String message, SecretKey key) async {
  final secretBox = await algorithm.encrypt(
    message.codeUnits,
    secretKey: key,
  );
  return secretBox.concatenation();
}

// Entschlüsseln
Future<String> decrypt(List<int> encrypted, SecretKey key) async {
  final secretBox = SecretBox.fromConcatenation(
    encrypted,
    nonceLength: 12,
    macLength: 16,
  );
  final decrypted = await algorithm.decrypt(secretBox, secretKey: key);
  return String.fromCharCodes(decrypted);
}
```

### Architektur-Diagramm (Konzept)

```
[Gerät A]                                    [Gerät B]
    |                                            |
    |-- 1. Signaling (einmalig, z.B. QR) ------>|
    |<-- 2. SDP Answer -------------------------|
    |                                            |
    |== 3. Direkte P2P-Verbindung (DTLS) =======|
    |                                            |
    |-- 4. Verschlüsselte Daten (E2E) --------->|
    |<-- 5. Verschlüsselte Antwort (E2E) -------|
```

**Keine zentrale Server-Beteiligung nach initialer Verbindung!**

### Wichtige Packages

```yaml
dependencies:
  flutter_webrtc: ^0.9.0        # WebRTC für Internet-P2P
  nearby_connections: ^3.0.0     # Lokales P2P (WiFi/Bluetooth)
  cryptography: ^2.7.0           # End-to-End-Verschlüsselung
  encrypt: ^5.0.0                # Alternative Verschlüsselung
```

### Best Practices

- **NAT-Traversierung**: STUN-Server nur für ICE-Candidates, kein Datendurchlauf
- **Verschlüsselung**: Doppelte Verschlüsselung (Transport + E2E)
- **Schlüsselaustausch**: Diffie-Hellman oder vorgenerierte Keys (QR-Code)
- **Fallback**: Bei NAT-Problemen optional TURN-Server (aber dann kein echtes P2P)

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
