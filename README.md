# Barbería Bonoso · PWA con Google Sheets + GitHub Pages

| Archivo | Para qué |
|---|---|
| `index.html` | La app completa: web pública, Mi cuenta y panel del barbero |
| `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png` | Lo que la hace instalable como app en el móvil |
| `Code.gs` | El "servidor": va dentro de la hoja de Google y crea toda la base de datos |

Cómo funciona: **GitHub Pages** sirve la app → la app habla con **Apps Script** → Apps Script lee y escribe en tu **Google Sheet**.
Los clientes nunca ven la hoja: solo tú.

---

## 1. Crear la base de datos (Google Sheets) — 5 min
1. Ve a sheets.new y crea una hoja llamada **Bonoso**.
2. **Extensiones → Apps Script**. Borra el código que aparece y pega todo `Code.gs`. Guarda (icono del disquete).
3. **Configuración del proyecto** (engranaje, menú izquierdo) → *Zona horaria*: **(GMT+01:00) Madrid**.
4. Arriba, en el desplegable de funciones, elige **instalar** y pulsa **Ejecutar**.
   Google pedirá permisos → *Revisar permisos* → tu cuenta → *Configuración avanzada* → *Ir a Bonoso (no seguro)* → *Permitir*.
   (Sale "no seguro" porque el script es tuyo y no está verificado por Google; es normal.)
5. Vuelve a la hoja: verás las pestañas **Citas, Usuarios, Visitas, Sesiones, Ajustes**.

## 2. Publicar el servidor — 2 min
1. En Apps Script: **Implementar → Nueva implementación** → engranaje → **Aplicación web**.
2. *Ejecutar como*: **Yo** · *Quién tiene acceso*: **Cualquier usuario**.
3. **Implementar** y copia la **URL de la aplicación web** (termina en `/exec`).

## 3. Conectar la app — 1 min
Abre `index.html`, busca `CONFIGURACIÓN` y pega la URL:
```js
const API_URL='https://script.google.com/macros/s/AKfy.../exec';
```

## 4. Publicar en GitHub Pages — 3 min
1. Crea un repositorio `bonoso` y sube: `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`.
   (**No subas `Code.gs`**: ya está en Google y no hace falta en GitHub.)
2. **Settings → Pages → Branch: main / (root) → Save**.
3. En 1–2 minutos: `https://TU-USUARIO.github.io/bonoso/`

## 5. Hacerte administrador
1. Abre la app → **Mi cuenta → Crear cuenta** con tu correo.
2. En la hoja, pestaña **Usuarios**, escribe `TRUE` en la columna **is_admin** de tu fila.
3. Entra en **Acceso barbero**. Para dar acceso al barbero: que cree su cuenta y pones `TRUE` en su fila.

## 6. Instalar en el móvil
- iPhone (Safari): Compartir → **Añadir a pantalla de inicio**
- Android (Chrome): ⋮ → **Instalar aplicación**

---

## Qué hace sola
- **Correos reales** desde tu Gmail (con el nombre "Barbería Bonoso"): bienvenida, confirmación de cita, recordatorio 24 h antes y código para cambiar la contraseña.
- **WhatsApp**: en el panel, cada cita de las próximas 48 h tiene un botón que abre WhatsApp con el recordatorio ya escrito. Enviarlos sin pulsar nada requiere la API de WhatsApp Business (de pago).
- **Validación en el servidor**: horario, festivos, días cerrados, franjas bloqueadas, horas pasadas y solapes. Dos personas no pueden coger el mismo hueco aunque reserven a la vez.
- **Contraseñas cifradas** (nunca se guardan en claro) y bloqueo tras 8 intentos fallidos.
- **Sesiones caducadas** se borran cada noche.

## Límites de Google (cuenta gratuita)
- **100 correos al día**. Para una barbería sobra; con Google Workspace son 1.500.
- Cada acción tarda **1–2 segundos** (es lo que tarda Google en responder).
- Los cambios de otros (nueva cita, retraso) aparecen en **~20 segundos**, no al instante.

## Cuidado con la hoja
- Puedes mirar y corregir datos a mano, pero **no cambies el orden ni el nombre de las columnas**.
- Las columnas `hash` y `salt` de **Usuarios** son las contraseñas cifradas: no las toques.
- **No compartas la hoja** con nadie que no deba ver los teléfonos y correos de los clientes.

## Actualizar
- **App**: sube el nuevo `index.html` a GitHub y cambia `bonoso-v1` por `bonoso-v2` en `sw.js` para que los móviles cojan la versión nueva.
- **Servidor**: pega el nuevo `Code.gs` → **Implementar → Gestionar implementaciones** → lápiz → *Versión: Nueva versión* → Implementar. La URL no cambia.
