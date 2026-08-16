# Chavarria Carpentry — Web Client Platform

[![Flutter Web](https://img.shields.io/badge/Flutter-Web_3.3%2B-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.3%2B_<_4.0-0175C2?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![Supabase](https://img.shields.io/badge/Backend-Supabase_BaaS-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com)
[![Netlify](https://img.shields.io/badge/Deployment-Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)](https://netlify.com)

Plataforma e-commerce progresiva diseñada para modernizar la presencia digital y el catálogo interactivo de una empresa de carpintería y diseño de muebles a la medida. Construida con Flutter Web y respaldada por arquitectura Serverless/BaaS con Supabase.

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
| **Seguridad de Cuenta:** Necesidad de verificar usuarios para evitar cuentas falsas en pedidos personalizados. | **Verificación OTP 2FA:** Integración de códigos de un solo uso por correo electrónico para el restablecimiento de contraseñas. |

---

## Arquitectura de Sistema

El sistema utiliza un enfoque **Decoupled Client-BaaS Architecture**:

```text
┌────────────────────────────────────────────────────────┐
│                   Cliente Web (Flutter)                │
│       [Compilado a WebAssembly / HTML5 en Netlify]     │
└────────────┬──────────────────────────────┬────────────┘
             │                              │
             ▼ REST / WebSocket             ▼ HTTP API
┌───────────────────────────┐    ┌───────────────────────────┐
│     Supabase Backend      │    │  SendGrid Email Service   │
│  • Auth (JWT)             │    │  • Envío de Códigos OTP   │
│  • PostgreSQL DB          │    │  • Verificación en 2 pasos│
│  • Storage (Imágenes)     │    └───────────────────────────┘
└───────────────────────────┘


Características y Módulos Destacados
Navegación y Filtros de Productos
Búsqueda Dinámica: Filtrado en tiempo real por palabra clave, nombre de producto o tipo de mueble.

Categorización Estructurada: Navegación por salas, dormitorios, comedores y trabajos a la medida.

Tarjetas Informativas: Visualización de imágenes en alta resolución, dimensiones y descripciones detalladas.

Autenticación y Seguridad
Registro e Inicio de Sesión: Gestión de usuarios integrada con la base de datos de Supabase Auth.

Protección de Carrito: Redirección automática hacia el módulo de Login/Register cuando un usuario no autenticado intenta añadir productos al carrito.

Módulo OTP (2FA): Lógica de verificación mediante SendGrid para el envío y validación de tokens de seguridad durante el restablecimiento de credenciales.

Desafíos Técnicos y Lecciones Aprendidas
Optimización de Flutter Web: Para garantizar una carga rápida en navegadores desktop y móviles, se aplicaron técnicas de optimización de imágenes y la gestión eficiente del árbol de widgets.

Manejo de Estados: Implementación de flujos reactivos para sincronizar el estado del carrito de compras con las búsquedas y filtros activos.

Gestión de Servicios de Terceros: Integración de proveedores de correo (SendGrid) y autenticación BaaS (Supabase) bajo arquitecturas asíncronas.

Configuración y Ejecución Local
Prerrequisitos
Flutter SDK: Version >=3.3.4 <4.0.0

Dart SDK: Version >=3.3.4

Google Chrome, Microsoft Edge o cualquier navegador moderno.


Pasos para Ejecutar
Clonar el repositorio:

Bash
git clone [https://github.com/tu-usuario/chavarria-carpentry-web.git](https://github.com/tu-usuario/chavarria-carpentry-web.git)
cd chavarria-carpentry-web
Instalar dependencias de Flutter:

Bash
flutter pub get
Configurar Variables de Entorno / Supabase:
Crea o edita el archivo de configuración en lib/config/supabase_config.dart con tus credenciales:

Dart
class SupabaseConfig {
  static const String url = 'TU_SUPABASE_URL';
  static const String anonKey = 'TU_SUPABASE_ANON_KEY';
}
Ejecutar en modo Desarrollo (Chrome):

Bash
flutter run -d chrome
Compilar para Producción:

Bash
flutter build web --release
Los archivos resultantes en build/web/ están listos para ser desplegados en Netlify, Vercel o GitHub Pages.
