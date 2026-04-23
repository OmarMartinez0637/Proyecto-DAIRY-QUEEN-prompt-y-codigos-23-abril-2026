¡Excelente elección! Para un desarrollador de Flutter, dominar el **Firebase CLI** es fundamental, ya que automatiza la configuración y evita errores manuales al conectar aplicaciones.

Aquí tienes la guía técnica paso a paso para preparar tu entorno en Windows.

---

## 1. Software Necesario: Node.js y NPM
Para usar Firebase CLI, necesitamos **Node.js**, que incluye automáticamente a **npm** (Node Package Manager).

### ¿Cómo verificar si ya los tienes?
Abre una terminal (PowerShell o CMD) y escribe:
1. `node -v`
2. `npm -v`

> Si ves un número de versión (ej. `v20.11.0`), ¡ya estás listo! Si aparece un error indicando que el comando no se reconoce, sigue los pasos de instalación.

---

## 2. Instalación Paso a Paso de Node.js (Windows)

Si no lo tienes instalado, sigue este procedimiento para asegurar una instalación global correcta:

1.  **Descarga:** Ve al sitio oficial [nodejs.org](https://nodejs.org/) y descarga la versión **LTS** (es la más estable para desarrollo).
2.  **Ejecución:** Abre el instalador `.msi`.
3.  **Configuración Clave:** * Acepta los términos.
    * En la pantalla de "Custom Setup", asegúrate de que la opción **"Add to PATH"** esté seleccionada (esto es lo que permite que sea "global").
4.  **Finalización:** Haz clic en "Install". Al terminar, **reinicia tu terminal** o tu computadora para que los cambios en las variables de entorno se apliquen.

---

## 3. Instalación de Firebase CLI (Global)

Una vez que `npm` funciona, instalaremos las herramientas de Firebase de forma global para que puedas usarlas en cualquier carpeta de tu proyecto `xflutteromar0637`.

### Comando de Instalación:
En tu terminal, ejecuta:
```bash
npm install -g firebase-tools
```
* El flag `-g` significa **Global**, permitiendo que el comando `firebase` sea reconocido en todo tu sistema.

---

## 4. Comandos Esenciales de Firebase CLI

Aquí es donde vinculamos tu entorno local con la nube de Google.

### Cómo acceder a Firebase con tu Cuenta de Google
Para "loguearte" y dar permiso a tu computadora de manejar tus proyectos:
```bash
firebase login
```
1.  Se abrirá una ventana en tu navegador predeterminado.
2.  Selecciona tu cuenta de Google (la misma que usaste para crear la base de datos `helados`).
3.  Haz clic en **Permitir**.
4.  En la terminal verás el mensaje: `✔  Success! Logged in as tu-correo@gmail.com`.

### Cómo usar firebase-tools (Comandos útiles)
* **Listar proyectos:** `firebase projects:list` (Verás tu proyecto de la heladería aquí).
* **Cerrar sesión:** `firebase logout`.

---

## 5. Configuración de Firebase para Flutter (FlutterFire)

Para que tu proyecto `cruddairyqueen` se conecte correctamente, el estándar profesional actual es usar el ejecutable de **FlutterFire**.

1.  **Instala el activador de FlutterFire:**
    ```bash
    dart pub global activate flutterfire_cli
    ```
2.  **Configura tu App:** Entra a tu carpeta `cruddairyqueen` y ejecuta:
    ```bash
    flutterfire configure
    ```
    Este comando es "mágico": te pedirá elegir tu proyecto de Firebase, detectará que es una app de Flutter y generará automáticamente el archivo `firebase_options.dart` con todas las credenciales de Android e iOS.

---

### Resumen de Flujo de Trabajo


1.  **Instalar Node.js** (Añadir al PATH).
2.  **Instalar Firebase Tools** (`npm install -g`).
3.  **Login** (`firebase login`).
4.  **Configurar Flutter** (`flutterfire configure`).

¿Lograste ver el número de versión de npm o necesitas ayuda con algún error específico durante la instalación?
