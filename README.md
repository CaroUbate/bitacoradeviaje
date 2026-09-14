# Bitácora Viajera — versión web (GitHub Pages)

Es un solo archivo (`index.html`), sin instalación ni paso de compilación.
Para que los gastos se compartan entre todos los que usen el link, usa una
base de datos gratuita en la nube (Firebase Firestore de Google). Nadie
necesita cuenta de Claude ni de Google para *usar* la app — la cuenta de
Google solo la creas tú, una vez, para prender la base de datos.

## Paso 1 — Crea el proyecto de Firebase (gratis, ~10 min)

1. Ve a https://console.firebase.google.com y entra con cualquier cuenta de Google.
2. "Add project" (Agregar proyecto) → ponle un nombre, por ejemplo `bitacora-viajera`.
   Puedes desactivar Google Analytics, no lo necesitas.
3. Cuando el proyecto esté listo, en el menú izquierdo entra a **Build → Firestore Database**.
4. Dale "Create database" → elige la ubicación más cercana (`southamerica-east1` o `us-central1`) → modo **"Start in test mode"**.
5. Ve a **Project settings** (el engranaje, arriba a la izquierda) → baja hasta "Your apps" → clic en el ícono `</>` (Web app) → ponle un nombre y "Register app".
6. Firebase te muestra un bloque `firebaseConfig = { apiKey: ..., ... }`. Copia esos 6 valores.

## Paso 2 — Pega la configuración en el archivo

Abre `index.html`, busca cerca del inicio este bloque:

```js
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROYECTO.firebaseapp.com",
  ...
};
```

Reemplaza cada valor por el que copiaste de Firebase. Guarda el archivo.

## Paso 3 — Reglas de acceso de Firestore

Por defecto, "test mode" deja de funcionar a los 30 días. Para que la app
funcione indefinidamente, en **Firestore Database → Rules**, pega esto y
dale "Publish":

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /trips/{tripId} {
      allow read, write: if true;
    }
  }
}
```

**Nota de seguridad:** esto deja los datos de viaje abiertos a quien conozca
el código de 6 caracteres (como una URL no listada) — no hay contraseñas
ni login. Es razonable para compartir gastos con amigos, pero no metas ahí
números de tarjeta ni datos sensibles.

## Paso 4 — Sube esto a GitHub Pages

1. Crea un repositorio nuevo en GitHub (puede ser público o privado si tienes plan pago).
2. Sube `index.html` (y este README si quieres) a la raíz del repo.
3. En el repo: **Settings → Pages** → en "Source" elige la rama `main` y carpeta `/ (root)` → Save.
4. GitHub te da una URL tipo `https://tu-usuario.github.io/tu-repo/` — ese es
   el link que le mandas a tus amigos. Tarda 1-2 minutos en activarse la
   primera vez.

## Uso

- Cualquiera que abra el link puede crear un viaje o unirse con un código.
- El código de 6 caracteres es lo que compartes para que otros vean/agreguen
  gastos al mismo viaje.
- La lista de "mis viajes" en la pantalla de inicio se guarda en el navegador
  de cada persona (no es compartida) — así cada quien ve los suyos.
- El export a Excel/.csv funciona igual que en la versión de Claude.
