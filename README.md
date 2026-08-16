# 🪚 Chavarria Carpentry — Web Client Platform

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
📦lib
 ┣ 📂Backend
 ┃ ┣ 📂pagos
 ┃ ┃ ┣ 📜.env
 ┃ ┃ ┣ 📜package.json
 ┃ ┃ ┗ 📜server.js
 ┃ ┣ 📜.env
 ┃ ┣ 📜index.js
 ┃ ┗ 📜package.json
 ┣ 📂data
 ┃ ┣ 📂models
 ┃ ┃ ┣ 📜carrito.dart
 ┃ ┃ ┣ 📜cart_item.dart
 ┃ ┃ ┣ 📜cliente.dart
 ┃ ┃ ┣ 📜envio.dart
 ┃ ┃ ┣ 📜productos.dart
 ┃ ┃ ┗ 📜users.dart
 ┃ ┗ 📂services
 ┃ ┃ ┣ 📜auth_service.dart
 ┃ ┃ ┣ 📜envio.dart
 ┃ ┃ ┣ 📜pedidos.dart
 ┃ ┃ ┣ 📜products_service.dart
 ┃ ┃ ┣ 📜searching_products.dart
 ┃ ┃ ┗ 📜ubicacion_service.dart
 ┣ 📂repositories
 ┃ ┣ 📜auth_repository.dart
 ┃ ┗ 📜productos_usuario.dart
 ┣ 📂viewmodels
 ┃ ┣ 📂auth
 ┃ ┃ ┣ 📜viewmodel_emailsend.dart
 ┃ ┃ ┣ 📜viewmodel_login.dart
 ┃ ┃ ┣ 📜viewmodel_password.dart
 ┃ ┃ ┗ 📜viewmodel_register.dart
 ┃ ┣ 📂home
 ┃ ┃ ┗ 📜home_viewmodel.dart
 ┃ ┣ 📂productos
 ┃ ┃ ┣ 📜carrito_viewmodel.dart
 ┃ ┃ ┗ 📜productos_viewmodel.dart
 ┃ ┗ 📂servicios
 ┃ ┃ ┣ 📜favoritos.dart
 ┃ ┃ ┣ 📜redireccionWoompi.dart
 ┃ ┃ ┗ 📜viewmodel_pedidos.dart
 ┣ 📂views
 ┃ ┣ 📂auth
 ┃ ┃ ┣ 📜view_olvidepass.dart
 ┃ ┃ ┣ 📜vista_login.dart
 ┃ ┃ ┗ 📜vista_register.dart
 ┃ ┗ 📂home
 ┃ ┃ ┣ 📂sections
 ┃ ┃ ┃ ┣ 📂conditions
 ┃ ┃ ┃ ┃ ┗ 📜terms_conditions.dart
 ┃ ┃ ┃ ┣ 📜carrito_detalle.dart
 ┃ ┃ ┃ ┣ 📜catalogos.dart
 ┃ ┃ ┃ ┣ 📜favoritos.dart
 ┃ ┃ ┃ ┣ 📜filtros.dart
 ┃ ┃ ┃ ┣ 📜info_producto.dart
 ┃ ┃ ┃ ┣ 📜pagocompleto.dart
 ┃ ┃ ┃ ┣ 📜perfil.dart
 ┃ ┃ ┃ ┣ 📜realizar_compra.dart
 ┃ ┃ ┃ ┣ 📜solicitar.dart
 ┃ ┃ ┃ ┗ 📜verificacionPago.dart
 ┃ ┃ ┣ 📂widgets
 ┃ ┃ ┃ ┣ 📂animations
 ┃ ┃ ┃ ┃ ┣ 📜carrusel_imagenes.dart
 ┃ ┃ ┃ ┃ ┗ 📜custom_chargin.dart
 ┃ ┃ ┃ ┣ 📜custom_APPBARUNIVERSAL.dart
 ┃ ┃ ┃ ┣ 📜custom_appBar_home.dart
 ┃ ┃ ┃ ┣ 📜custom_carrusel copy.dart
 ┃ ┃ ┃ ┣ 📜custom_carrusel.dart
 ┃ ┃ ┃ ┣ 📜custom_enviarCategorias.dart
 ┃ ┃ ┃ ┣ 📜custom_envio.dart
 ┃ ┃ ┃ ┣ 📜custom_footer.dart
 ┃ ┃ ┃ ┣ 📜custom_historialEnvios.dart
 ┃ ┃ ┃ ┣ 📜custom_showdialog.dart
 ┃ ┃ ┃ ┣ 📜custom_tomarProductos.dart
 ┃ ┃ ┃ ┣ 📜historialpedidos.dart
 ┃ ┃ ┃ ┣ 📜popup.dart
 ┃ ┃ ┃ ┗ 📜popup2.dart
 ┃ ┃ ┗ 📜home_screen.dart
 ┣ 📜main.dart
 ┣ 📜rutas.dart
 ┗ 📜supabase.dart
