pero el readme anterior tenia esto que me gusto bastante: # 🪚 Chavarria Carpentry — Web Client Platform

[![Flutter Web](https://img.shields.io/badge/Flutter-Web_3.3%2B-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.3%2B_<_4.0-0175C2?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![Node.js](https://img.shields.io/badge/Backend-Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org)
[![Supabase](https://img.shields.io/badge/BaaS-Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com)
[![Netlify](https://img.shields.io/badge/Deployment-Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)](https://netlify.com)

Plataforma e-commerce progresiva diseñada para modernizar la presencia digital y el catálogo interactivo de una empresa de carpintería y diseño de muebles a la medida. El proyecto utiliza un patrón de arquitectura desacoplado **MVVM (Model-View-ViewModel)** en el frontend con Flutter, integrando servicios Serverless con Supabase y un microservicio en Node.js para el procesamiento seguro de pagos.

---

## Demo en Vivo

> **Ecosistema Web:** [Explorar Plataforma en Netlify](https://684266f214b54400085c7fd1--webchavarria.netlify.app/)
>
> ⚠️ **Estado Actual del Servicio:** La interfaz de usuario y la navegación del catálogo están completamente funcionales. Las llamadas a la API de datos y el flujo de autenticación en tiempo real están temporalmente pausados debido a la caducidad de la capa gratuita del servidor de base de datos.

---

## El Problema vs. La Solución

| El Reto del Negocio | La Solución Tecnológica |
| :--- | :--- |
| **Catálogo Estático:** Los clientes requerían atención directa por mensajes para conocer precios, medidas y opciones de muebles. | **E-Commerce Interactivo:** Experiencia web fluida con búsqueda, filtrado por categorías de madera/estilo y fichas informativas detalladas. |
| **Fricción en Pedidos:** Falta de un flujo estructurado de compra para registrar las intenciones de pedido. | **Carrito & Autenticación:** Flujo inteligente que permite explorar libremente y solicita autenticación únicamente al momento de concretar la orden. |
| **Seguridad y Pagos:** Necesidad de validar transacciones e integrar un canal seguro para la confirmación de compras. | **Integración de Pasarela:** Servicio backend embebido para la redirección y verificación del proceso de cobro (Woompi / Gateway). |

---

## Arquitectura de Sistema

El sistema sigue una estructura **Clean MVVM Architecture** combinada con servicios backend desacoplados:

```text
┌─────────────────────────────────────────────────────────────────┐
│                      CAPA DE VISTA (Views)                      │
│            Widgets UI, Diálogos, Pantallas & Formularios        │
└────────────────────────────────┬────────────────────────────────┘
                                 │ Observa estados (Notifier / Reactive)
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│                 CAPA DE NEGOCIO (ViewModels)                    │
│    Gestión de estado de Auth, Carrito, Productos y Servicios    │
└────────────────┬────────────────────────────────┬───────────────┘
                 │                                │
                 ▼                                ▼
┌─────────────────────────────────┐  ┌────────────────────────────┐
│   REPOSITORIOS (Repositories)   │  │    SERVICIOS (Services)   │
│ Abstracción y acceso a datos    │  │ Consumo de APIs y Auth     │
└────────────────┬────────────────┘  └────────────┬───────────────┘
                 │                                │
                 └────────────────┬───────────────┘
                                  ▼
┌─────────────────────────────────────────────────────────────────┐
│                    CAPA DE DATOS (Data/Backend)                 │
│    • Supabase BaaS (Auth, PostgreSQL, Storage)                  │
│    • Node.js Backend (Servicio dedicado para pagos)             │
└─────────────────────────────────────────────────────────────────┘
```

Estructura del Código y Explicación por Capas
A continuación se detalla la responsabilidad técnica de cada directorio dentro de la aplicación:
# Arquitectura de Archivos (`lib/`)

```text
📦 lib
 ┣ 📂 Backend                 # Microservicios backend embebidos (Node.js)
 ┃ ┣ 📂 pagos                 # Módulo de integración de pasarela de pagos
 ┃ ┃ ┣ 📜 .env                # Variables de entorno privadas para llaves de pago
 ┃ ┃ ┣ 📜 package.json        # Dependencias de Node.js para transacciones
 ┃ ┃ ┗ 📜 server.js           # Servidor HTTP para la pasarela de pago
 ┃ ┣ 📜 .env                  # Configuración de servidor general
 ┃ ┣ 📜 index.js              # Punto de entrada de la API backend secundaria
 ┃ ┗ 📜 package.json          # Configuración de dependencias de API
 ┣ 📂 data                    # Capa de Datos Puro (Entidades e Integraciones)
 ┃ ┣ 📂 models                # Modelos de datos y mapeo JSON/Dart
 ┃ ┃ ┣ 📜 carrito.dart        # Estructura del objeto de carrito general
 ┃ ┃ ┣ 📜 cart_item.dart      # Mapeo de elementos individuales del carrito
 ┃ ┃ ┣ 📜 cliente.dart        # Modelo de entidad de cliente
 ┃ ┃ ┣ 📜 envio.dart          # Estructura para datos de despacho/dirección
 ┃ ┃ ┣ 📜 productos.dart      # Modelo de productos y categorías de muebles
 ┃ ┃ ┗ 📜 users.dart          # Modelo de perfil de usuario
 ┃ ┗ 📂 services              # Conectores y APIs de bajo nivel
 ┃ ┃ ┣ 📜 auth_service.dart       # Conexión directa con Supabase Auth
 ┃ ┃ ┣ 📜 envio.dart              # Lógica para cálculo y registro de envíos
 ┃ ┃ ┣ 📜 pedidos.dart            # Endpoints para creación de órdenes de compra
 ┃ ┃ ┣ 📜 products_service.dart         # Consultas de productos a la base de datos
 ┃ ┃ ┣ 📜 searching_products.dart       # Servicio de filtrado y búsqueda en tiempo real
 ┃ ┃ ┗ 📜 ubicacion_service.dart       # Manejo de direcciones y geolocalización
 ┣ 📂 repositories                  # Capa de Abstracción de Datos
 ┃ ┣ 📜 auth_repository.dart       # Repositorio puente para manejo de sesiones
 ┃ ┗ 📜 productos_usuario.dart     # Gestión de productos asignados/favoritos del usuario
 ┣ 📂 viewmodels                    # Capa de Lógica de Negocio y Estado (MVVM)
 ┃ ┣ 📂 auth                  # Control de estado de autenticación
 ┃ ┃ ┣ 📜 viewmodel_emailsend.dart      # Envío de correos/OTP para verificación
 ┃ ┃ ┣ 📜 viewmodel_login.dart          # Estado e interacción de Inicio de Sesión
 ┃ ┃ ┣ 📜 viewmodel_password.dart       # Lógica de recuperación de contraseña
 ┃ ┃ ┗ 📜 viewmodel_register.dart       # Flujo de creación de cuentas de usuario
 ┃ ┣ 📂 home                        # Estado de la vista principal
 ┃ ┃ ┗ 📜 home_viewmodel.dart     # Carga reactiva de banners, listas y destacados
 ┃ ┣ 📂 productos                   # Gestión de productos e inventario reactivo
 ┃ ┃ ┣ 📜 carrito_viewmodel.dart     # Lógica del carrito (añadir, eliminar, totales)
 ┃ ┃ ┗ 📜 productos_viewmodel.dart     # Estado de filtros, categorías y catálogo
 ┃ ┗ 📂 servicios                       # ViewModels para flujos adicionales
 ┃ ┃ ┣ 📜 favoritos.dart                # Gestión reactiva de productos guardados
 ┃ ┃ ┣ 📜 redireccionWoompi.dart         # Orquestación y respuesta del flujo de pago
 ┃ ┃ ┗ 📜 viewmodel_pedidos.dart         # Lógica para el seguimiento de órdenes creadas
 ┣ 📂 views                               # Capa de Interfaz de Usuario (UI)
 ┃ ┣ 📂 auth                              # Pantallas de acceso e identidad
 ┃ ┃ ┣ 📜 view_olvidepass.dart           # Vista de recuperación de credenciales
 ┃ ┃ ┣ 📜 vista_login.dart                # Interface de inicio de sesión
 ┃ ┃ ┗ 📜 vista_register.dart             # Interface de registro de nuevo cliente
 ┃ ┗ 📂 home                                # Pantallas y secciones de la aplicación
 ┃ ┃ ┣ 📂 sections                      # Subvistas y páginas principales
 ┃ ┃ ┃ ┣ 📂 conditions                   # Políticas legales
 ┃ ┃ ┃ ┃ ┗ 📜 terms_conditions.dart       # Pantalla de Términos y Condiciones
 ┃ ┃ ┃ ┣ 📜 carrito_detalle.dart         # Resumen final de la compra
 ┃ ┃ ┃ ┣ 📜 catalogos.dart                # Vista general del catálogo interactivo
 ┃ ┃ ┃ ┣ 📜 favoritos.dart                # Pantalla de artículos guardados
 ┃ ┃ ┃ ┣ 📜 filtros.dart                  # Panel de filtrado avanzado
 ┃ ┃ ┃ ┣ 📜 info_producto.dart             # Ficha técnica y detalle del mueble
 ┃ ┃ ┃ ┣ 📜 pagocompleto.dart             # Confirmación de pago exitoso
 ┃ ┃ ┃ ┣ 📜 perfil.dart                   # Ajustes de perfil e historial del usuario
 ┃ ┃ ┃ ┣ 📜 realizar_compra.dart           # Checkout y selección de envío
 ┃ ┃ ┃ ┣ 📜 solicitar.dart                  # Formulario para muebles a la medida
 ┃ ┃ ┃ ┗ 📜 verificacionPago.dart           # Comprobación de estado de transacción
 ┃ ┃ ┣ 📂 widgets                             # Componentes UI reutilizables
 ┃ ┃ ┃ ┣ 📂 animations                        # Componentes visuales animados
 ┃ ┃ ┃ ┃ ┣ 📜 carrusel_imagenes.dart         # Slider dinámico de productos
 ┃ ┃ ┃ ┃ ┗ 📜 custom_chargin.dart           # Loader personalizado de la marca
 ┃ ┃ ┃ ┣ 📜 custom_APPBARUNIVERSAL.dart       # Barra de navegación secundaria
 ┃ ┃ ┃ ┣ 📜 custom_appBar_home.dart       # Header principal con acceso a perfil/carrito
 ┃ ┃ ┃ ┣ 📜 custom_carrusel.dart         # Carrusel promocional
 ┃ ┃ ┃ ┣ 📜 custom_enviarCategorias.dart       # Selector rápido de categorías
 ┃ ┃ ┃ ┣ 📜 custom_envio.dart         # Formulario de dirección de entrega
 ┃ ┃ ┃ ┣ 📜 custom_footer.dart         # Pie de página institucional
 ┃ ┃ ┃ ┣ 📜 custom_historialEnvios.dart       # Lista de seguimientos
 ┃ ┃ ┃ ┣ 📜 custom_showdialog.dart         # Modales de notificación estándar
 ┃ ┃ ┃ ┣ 📜 custom_tomarProductos.dart      # Tarjeta de producto individual
 ┃ ┃ ┃ ┣ 📜 historialpedidos.dart          # Tabla/Lista de compras pasadas
 ┃ ┃ ┃ ┣ 📜 popup.dart           # Diálogos emergentes
 ┃ ┃ ┃ ┗ 📜 popup2.dart          # Confirmaciones secundarias
 ┃ ┃ ┗ 📜 home_screen.dart        # Contenedor principal de la pantalla de inicio
 ┣ 📜 main.dart               # Punto de entrada de la aplicación Flutter
 ┣ 📜 rutas.dart              # Mapeo de rutas de navegación de la aplicación
 ┗ 📜 supabase.dart           # Inicialización y cliente singleton de Supabase
```
## Características Principales

### Catálogo e Interacción
* **Navegación Dinámica:** Exploración de muebles por categorías con tarjetas interactivas (`custom_tomarProductos.dart`).
* **Búsqueda Reactiva:** Filtrado en tiempo real procesado desde la capa de servicios (`searching_products.dart`).
* **Solicitudes Personalizadas:** Módulo dedicado para cotizar trabajos de carpintería a la medida (`solicitar.dart`).

### Carrito y Pagos Integrados
* **Estado del Carrito:** Actualización en tiempo real mediante `carrito_viewmodel.dart`.
* **Procesamiento de Pagos:** Integración con backend Node.js dedicado para la verificación de transacciones (`redireccionWoompi.dart`).
* **Seguimiento de Envíos:** Gestión de direcciones e historial de órdenes asociadas al perfil de usuario.

---

## Configuración y Ejecución Local

### Prerrequisitos
* **Flutter SDK:** Version `>=3.3.4 <4.0.0`
* **Node.js:** `>=18.x` (para ejecutar el servicio de pagos en `lib/Backend/pagos`)
* **Navegador:** Google Chrome o navegador compatible con Flutter Web.

---

### Pasos de Ejecución

#### 1. Clonar el repositorio
```bash
git clone [https://github.com/Gerson-dev11/chavarria-carpentry-web.git](https://github.com/Gerson-dev11/chavarria-carpentry-web.git)
cd chavarria-carpentry-web
```

#### 2. Configurar el Backend de Pagos (Node.js)
```bash
cd lib/Backend/pagos
npm install
```

Configura el archivo .env con tus credenciales de pago.
Inicia el servicio:
```Bash
npm start
```

#### 3. Ejecutar la Aplicación Web (Flutter)
Regresa a la raíz del proyecto, instala las dependencias de Dart e inicia la aplicación:

```Bash
flutter pub get
flutter run -d chrome
```
