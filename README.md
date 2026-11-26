# Hextock — Plataforma de E-commerce e Inventarios
**Hextock** es una aplicación web diseñada para ayudar a pequeños y medianos comercios a centralizar su inventario, rescatar catálogos desordenados y sincronizar existencias con Shopify mediante un proceso guiado en tres etapas (Migración, Rescate y Acompañamiento mensual).

**Deploy en vivo:** https://aat-mockup-brandon.vercel.app

---

## Estructura del proyecto

- `index.html`: Página principal (landing page) que presenta la propuesta de valor de Hextock, el problema de desincronización de inventarios y el acceso al flujo de registro y planes.
- `Registro.html`: Formulario de creación de cuenta para nuevos comercios, conectado con Supabase Auth para registrar usuarios y redirigir al catálogo de planes.
- `Login.html`: Vista de inicio de sesión para usuarios registrados, validando credenciales contra Supabase antes de permitir la contratación de paquetes.
- `Planes.html`: Catálogo interactivo con las tres fases del servicio (Fase 1: Migración a Shopify, Fase 2: Rescate de Inventario y Fase 3: Acompañamiento Mensual), verificación de sesión activa y registro de suscripciones en Supabase.
- `PROMPT_LOG.md`: Bitácora de prompts e iteraciones realizadas durante el desarrollo con asistencia de inteligencia artificial.

---

## Cómo correr y probar el flujo

1. Abre la aplicación desplegada en **Vercel** (`https://aat-mockup-brandon.vercel.app`) o abre `index.html` en un servidor local (por ejemplo, con Live Server en VS Code).
2. Haz clic en **Comenzar / Registrarse** para ir a `Registro.html` y crear una cuenta de usuario, o entra a `Login.html` si ya tienes credenciales registradas.
3. Una vez con sesión iniciada, navega a `Planes.html` para revisar las tres etapas del servicio y seleccionar el paquete o suscripción correspondiente.

---

## Decisiones de diseño vs. sugerencias del IS

### 1. Seguridad y control de acceso en frontend estático (Supabase Auth + RLS)
- **Qué propuso el sistema inteligente:** Conectar directamente el frontend estático (`Registro.html`, `Login.html` y `Planes.html`) mediante el SDK público de Supabase en el navegador y permitir la selección libre de paquetes desde la interfaz.
- **Qué decidí:** **Revisar y restringir** el flujo exigiendo verificación explícita de sesión activa (`auth.getUser()`) antes de ejecutar cualquier escritura de suscripción en `Planes.html`, además de limitar el cliente web únicamente al uso de la `anon public key` respaldada por políticas de Row Level Security (RLS) en Supabase.
- **Por qué:** Al ser una arquitectura frontend sin backend intermedio propio, exponer escrituras sin validar sesión activa permitiría registros huérfanos o manipulación de suscripciones desde el navegador; exigir autenticación previa y RLS protege la integridad de los datos por usuario.

### 2. Escalabilidad del modelo de sincronización e incorporación por fases
- **Qué propuso el sistema inteligente:** Construir desde la primera versión un tablero SaaS 100% autoservicio con sincronización automática y bidireccional en tiempo real contra la API de Shopify para cualquier comercio que se registrara.
- **Qué decidí:** **Rechazar** la automatización masiva inicial y estructurar `Planes.html` en tres fases secuenciales (Migración, Rescate de datos y Acompañamiento mensual).
- **Por qué:** Automatizar sincronizaciones en vivo sobre catálogos brutos sin limpiar SKUs dispararía errores de API, duplicados y consumo innecesario de cuota en Supabase y Shopify; separar la limpieza e incorporación por fases reduce fallos operativos y permite escalar de forma controlada.

### 3. Retroalimentación de estado en tiempo real en `Planes.html`
- **Qué propuso el sistema inteligente:** Mantener únicamente alertas básicas del navegador (`alert()`) cuando el usuario intentaba contratar un plan sin sesión o al consultar el estado de autenticación.
- **Qué decidí:** **Aceptar con ajustes**, conservando las validaciones visuales claras para el usuario final y documentando el estado de verificación de sesión durante las pruebas.
- **Por qué:** Facilita diagnosticar rápidamente si la sesión de Supabase está activa antes de intentar guardar una suscripción, evitando fallos silenciosos en el flujo entre `Login.html`, `Registro.html` y `Planes.html`.

---

## Contexto del historial de commits

Durante el desarrollo iterativo se realizaron 38 commits en la rama `principal`. Algunos mensajes tempranos (como `Crear Login.html`, `Actualizar Planes.html` o `Actualizar index.html`) corresponden a ajustes incrementales de estructura HTML, enlace de navegación entre vistas, integración de estilos responsivos y conexión inicial con el cliente de Supabase antes de consolidar la versión final desplegada en Vercel.
