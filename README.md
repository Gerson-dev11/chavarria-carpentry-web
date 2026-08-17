# 🪵 Chavarria Carpentry — Plataforma E-commerce

Plataforma web para una empresa de carpintería y fabricación de muebles a medida.

Permite explorar el catálogo, buscar y filtrar productos, gestionar un carrito, solicitar muebles personalizados, realizar pedidos y procesar pagos.

### 🔗 Enlaces

**[🌐 Demo](https://684266f214b54400085c7fd1--webchavarria.netlify.app/)** · **[💻 GitHub](https://github.com/Gerson-dev11/chavarria-carpentry-web)**

> ⚠️ La interfaz y navegación del catálogo están disponibles en la demo. Las funcionalidades que dependen de Supabase requieren actualmente una instancia activa de la base de datos.

---

## 🛠️ Tecnologías

**Frontend:** Flutter Web · Dart
**Arquitectura:** MVVM · Repository Pattern
**Backend:** Node.js
**Base de datos:** PostgreSQL / Supabase
**Autenticación:** Supabase Auth
**Storage:** Supabase Storage
**Pagos:** Wompi / Payment Gateway
**Deploy:** Netlify

---

## 📁 Estructura del proyecto

```text
lib/
│
├── Backend/
│   ├── pagos/
│   │   ├── server.js
│   │   └── package.json
│   ├── index.js
│   └── package.json
│
├── data/
│   ├── models/
│   └── services/
│
├── repositories/
│
├── viewmodels/
│   ├── auth/
│   ├── home/
│   ├── productos/
│   └── servicios/
│
├── views/
│   ├── auth/
│   └── home/
│       ├── sections/
│       ├── widgets/
│       └── home_screen.dart
│
├── main.dart
├── rutas.dart
└── supabase.dart
```

### Capas principales

| Capa             | Responsabilidad                                 |
| ---------------- | ----------------------------------------------- |
| `views/`         | Interfaz, pantallas y componentes               |
| `viewmodels/`    | Estado y lógica de presentación                 |
| `repositories/`  | Abstracción del acceso a datos                  |
| `data/services/` | Consultas y comunicación con servicios externos |
| `data/models/`   | Modelos de datos                                |
| `Backend/`       | Servicios Node.js y procesamiento de pagos      |

---

## ✨ Funcionalidades

* 🪑 Catálogo de muebles y categorías
* 🔎 Búsqueda y filtrado de productos
* 📄 Detalle de productos
* 🛒 Carrito de compras
* 🔐 Registro e inicio de sesión
* 📧 Recuperación y verificación de cuenta
* ❤️ Productos favoritos
* 🪚 Solicitudes de muebles a medida
* 📦 Creación y seguimiento de pedidos
* 🚚 Gestión de direcciones y envíos
* 💳 Integración con pasarela de pagos

---

## 🏗️ Arquitectura

```text
                    Flutter Web
                         │
                         ▼
                    ┌─────────┐
                    │  Views  │
                    └────┬────┘
                         │
                         ▼
                  ┌──────────────┐
                  │  ViewModels  │
                  └──────┬───────┘
                         │
                 ┌───────┴────────┐
                 ▼                ▼
          ┌────────────┐   ┌────────────┐
          │Repositories│   │  Services  │
          └──────┬─────┘   └──────┬─────┘
                 │                │
                 └───────┬────────┘
                         ▼
              ┌─────────────────────┐
              │      Supabase       │
              │ Auth · PostgreSQL   │
              │      · Storage      │
              └─────────────────────┘

                         │
                         │ Pagos
                         ▼
                  ┌────────────┐
                  │  Node.js   │
                  │  Backend   │
                  └─────┬──────┘
                        ▼
                 Payment Gateway
```

### Flujo principal

```text
Catálogo
   ↓
Producto
   ↓
Carrito
   ↓
Checkout
   ↓
Autenticación
   ↓
Envío
   ↓
Pago
   ↓
Node.js
   ↓
Pasarela de pago
   ↓
Verificación
   ↓
Pedido
```

---

## 🚀 Ejecución local

### Requisitos

* Flutter `>=3.3.4 <4.0.0`
* Node.js `>=18.x`
* npm
* Chrome

### 1. Clonar

```bash
git clone https://github.com/Gerson-dev11/chavarria-carpentry-web.git
cd chavarria-carpentry-web
```

### 2. Instalar dependencias de Flutter

```bash
flutter pub get
```

### 3. Configurar Supabase

Configura las credenciales de tu instancia de Supabase según la configuración utilizada por el proyecto.

### 4. Instalar el backend de pagos

```bash
cd lib/Backend/pagos
npm install
```

Configura las variables necesarias en `.env` y ejecuta:

```bash
npm start
```

### 5. Ejecutar Flutter Web

Desde la raíz del proyecto:

```bash
flutter run -d chrome
```

---

## 🔐 Variables de entorno

Utiliza un `.env.example` como referencia para configurar las credenciales necesarias.

```env
SUPABASE_URL=
SUPABASE_ANON_KEY=

PAYMENT_API_KEY=
PAYMENT_SECRET=
```

**No subir credenciales reales al repositorio.**

---

## ⚠️ Estado actual

| Funcionalidad        | Estado                                |
| -------------------- | ------------------------------------- |
| Interfaz Web         | ✅                                     |
| Catálogo             | ✅                                     |
| Navegación           | ✅                                     |
| Carrito              | ✅                                     |
| Integración Supabase | ⚠️ Requiere instancia activa          |
| Autenticación        | ⚠️ Requiere instancia activa          |
| Pagos                | ⚠️ Requiere configuración del backend |

La demo desplegada permite revisar principalmente la interfaz y navegación. Las funcionalidades conectadas a Supabase deben configurarse nuevamente para ejecutarse en un entorno local.

---

## 👨‍💻 Autor

**Gerson Wilfredo Franco Gámez**

Junior Full Stack Developer · Backend-focused

[GitHub](https://github.com/Gerson-dev11) · [LinkedIn](https://www.linkedin.com/in/gerson-dev/)
