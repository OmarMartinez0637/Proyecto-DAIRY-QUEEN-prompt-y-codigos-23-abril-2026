¡Hola! Qué buen proyecto para una práctica guiada. Como desarrollador, te presento un plan de trabajo estructurado, desde la configuración de archivos hasta la lógica de agentes para que los estudiantes no solo programen, sino que entiendan la arquitectura del software.

Usaremos **Antigravity** (un enfoque de Clean Architecture/Modularización rápida) para mantener el código organizado.

---

## 1. Estructura de Trabajo y Preparación
Primero, organizamos el entorno local. En tu terminal o explorador de archivos:

* **Ruta principal:** `xflutteromar0637/`
* **Proyecto Flutter:** `cruddairyqueen`

```bash
mkdir xflutteromar0637
cd xflutteromar0637
flutter create cruddairyqueen
```

---

## 2. Configuración de Firebase y Dependencias
Para que el proyecto funcione, debemos vincularlo a la consola de Firebase.

### Paso A: Firestore
1. Ve a [Firebase Console](https://console.firebase.google.com/).
2. Crea un proyecto llamado **HeladeriaCRUD**.
3. En el menú lateral, selecciona **Firestore Database**.
4. Haz clic en **Crear base de datos** y nómbrala `helados`.
5. Define la colección `helados` con los campos: `nombre` (string), `precio` (number) y `stock` (number).

### Paso B: Modificación de `pubspec.yaml`
Abre el archivo y agrega las librerías necesarias bajo `dependencies`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.24.2
  cloud_firestore: ^4.14.0
  cupertino_icons: ^1.0.6
```
*Ejecuta `flutter pub get` en la terminal para instalarlas.*

---

## 3. Metodología de Agentes y Flujo de Trabajo
Para la práctica guiada, dividiremos la lógica en **Agentes** (Roles) para que los alumnos identifiquen responsabilidades:

| Agente | Rol | Skill (Habilidad) |
| :--- | :--- | :--- |
| **Data Agent** | Repositorio | Conexión directa con Firebase (CRUD puro). |
| **Logic Agent** | Controlador | Manejo de estados y validación de datos. |
| **UI Agent** | Vista | Presentación visual con colores atractivos. |

### Estructura de Carpetas (Antigravity Style)
```text
lib/
├── agents/
│   ├── data_agent.dart    (Conexión Firebase)
│   ├── logic_agent.dart   (Lógica de negocio)
├── ui/
│   ├── home_page.dart     (Lista de helados)
│   ├── ice_cream_form.dart(Formulario)
└── main.dart              (Punto de entrada)
```

---

## 4. Implementación del Código (Totalmente Funcional)

### A. Inicialización (`main.dart`)
```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'ui/home_page.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(const DairyQueenApp());
}

class DairyQueenApp extends StatelessWidget {
  const DairyQueenApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Dairy Queen CRUD',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.pink, // Color atractivo
        useMaterial3: true,
      ),
      home: const HomePage(),
    );
  }
}
```

### B. El Data Agent (CRUD) (`agents/data_agent.dart`)
```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class DataAgent {
  final CollectionReference helados = FirebaseFirestore.instance.collection('helados');

  // CREATE
  Future<void> addHelado(String nombre, double precio, int stock) {
    return helados.add({
      'nombre': nombre,
      'precio': precio,
      'stock': stock,
    });
  }

  // READ (Stream para tiempo real)
  Stream<QuerySnapshot> getHelados() {
    return helados.snapshots();
  }

  // UPDATE
  Future<void> updateHelado(String docId, String nombre, double precio, int stock) {
    return helados.doc(docId).update({
      'nombre': nombre,
      'precio': precio,
      'stock': stock,
    });
  }

  // DELETE
  Future<void> deleteHelado(String docId) {
    return helados.doc(docId).delete();
  }
}
```

### C. La Interfaz de Usuario (`ui/home_page.dart`)
Utilizaremos colores vibrantes como rosa suave y azul cian para un look de heladería.

```dart
import 'package:flutter/material.dart';
import 'package:cloud_firestore/cloud_firestore.dart';
import '../agents/data_agent.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final DataAgent _agent = DataAgent();
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _priceController = TextEditingController();
  final TextEditingController _stockController = TextEditingController();

  void _showForm(String? docId, [String? n, double? p, int? s]) {
    if (docId != null) {
      _nameController.text = n!;
      _priceController.text = p.toString();
      _stockController.text = s.toString();
    } else {
      _nameController.clear();
      _priceController.clear();
      _stockController.clear();
    }

    showModalBottomSheet(
      context: context,
      isScrollControlled: true,
      builder: (_) => Padding(
        padding: EdgeInsets.only(bottom: MediaQuery.of(context).viewInsets.bottom, left: 20, right: 20, top: 20),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(controller: _nameController, decoration: const InputDecoration(labelText: 'Nombre del Helado')),
            TextField(controller: _priceController, decoration: const InputDecoration(labelText: 'Precio'), keyboardType: TextInputType.number),
            TextField(controller: _stockController, decoration: const InputDecoration(labelText: 'Stock'), keyboardType: TextInputType.number),
            const SizedBox(height: 20),
            ElevatedButton(
              style: ElevatedButton.styleFrom(backgroundColor: Colors.pinkAccent, foregroundColor: Colors.white),
              onPressed: () async {
                if (docId == null) {
                  await _agent.addHelado(_nameController.text, double.parse(_priceController.text), int.parse(_stockController.text));
                } else {
                  await _agent.updateHelado(docId, _nameController.text, double.parse(_priceController.text), int.parse(_stockController.text));
                }
                Navigator.of(context).pop();
              },
              child: Text(docId == null ? 'Agregar' : 'Actualizar'),
            )
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('🍦 Dairy Queen Inventory'), backgroundColor: Colors.cyan[100]),
      body: StreamBuilder(
        stream: _agent.getHelados(),
        builder: (context, AsyncSnapshot<QuerySnapshot> snapshot) {
          if (!snapshot.hasData) return const Center(child: CircularProgressIndicator());
          return ListView.builder(
            itemCount: snapshot.data!.docs.length,
            itemBuilder: (context, index) {
              var doc = snapshot.data!.docs[index];
              return Card(
                color: Colors.pink[50],
                margin: const EdgeInsets.all(10),
                child: ListTile(
                  title: Text(doc['nombre'], style: const TextStyle(fontWeight: FontWeight.bold)),
                  subtitle: Text('Precio: \$${doc['precio']} | Stock: ${doc['stock']}'),
                  trailing: SizedBox(
                    width: 100,
                    child: Row(
                      children: [
                        IconButton(icon: const Icon(Icons.edit, color: Colors.blue), onPressed: () => _showForm(doc.id, doc['nombre'], doc['precio'].toDouble(), doc['stock'])),
                        IconButton(icon: const Icon(Icons.delete, color: Colors.red), onPressed: () => _agent.deleteHelado(doc.id)),
                      ],
                    ),
                  ),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.pinkAccent,
        onPressed: () => _showForm(null),
        child: const Icon(Icons.add, color: Colors.white),
      ),
    );
  }
}
```

---

## 5. Guía para el Estudiante (Flujo de Trabajo)
1.  **Observar:** Mira cómo los datos fluyen desde Firestore (Nube) hasta el `StreamBuilder` (Interfaz).
2.  **Actuar:** Al presionar el botón "Agregar", el **Data Agent** envía un mapa de datos a Firebase.
3.  **Reflexionar:** ¿Por qué usamos `snapshots()`? Porque permite que, si alguien cambia el precio en la consola de Firebase, la app se actualice sola sin refrescar.
