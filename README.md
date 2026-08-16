# 🪚 Chavarria Carpentry — Web Client Platform

[![Flutter Web](https://img.shields.io/badge/Flutter-Web_3.3%2B-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.3%2B_<_4.0-0175C2?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![Supabase](https://img.shields.io/badge/Backend-Supabase_BaaS-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com)
[![Netlify](https://img.shields.io/badge/Deployment-Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)](https://netlify.com)

Plataforma e-commerce progresiva diseñada para modernizar la presencia digital y el catálogo interactivo de una empresa de carpintería y diseño de muebles a la medida. Construida con Flutter Web y respaldada por arquitectura Serverless/BaaS con Supabase.

---

## 🔗 Demo en Vivo

> 🌐 **Ecosistema Web:** [Explorar Plataforma en Netlify](https://684266f214b54400085c7fd1--webchavarria.netlify.app/)
>
> ⚠️ **Estado Actual del Servicio:** La interfaz de usuario y la navegación del catálogo están completamente funcionales. Las llamadas a la API de datos y el flujo de autenticación en tiempo real están temporalmente pausados debido a la caducidad de la capa gratuita del servidor de base de datos.

---

## 💡 El Problema vs. La Solución

| El Reto del Negocio | La Solución Tecnológica |
| :--- | :--- |
| **Catálogo Estático:** Los clientes requerían atención directa por mensajes para conocer precios, medidas y opciones de muebles. | **E-Commerce Interactivo:** Experiencia web fluida con búsqueda, filtrado por categorías de madera/estilo y fichas informativas detalladas. |
| **Fricción en Pedidos:** Falta de un flujo estructurado de compra para registrar las intenciones de pedido. | **Carrito & Autenticación:** Flujo inteligente que permite explorar libremente y solicita autenticación únicamente al momento de concretar la orden. |
| **Seguridad de Cuenta:** Necesidad de verificar usuarios para evitar cuentas falsas en pedidos personalizados. | **Verificación OTP 2FA:** Integración de códigos de un solo uso por correo electrónico para el restablecimiento de contraseñas. |

---

## 🛠️ Arquitectura de Sistema

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
