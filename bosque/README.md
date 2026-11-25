# 🌲 Bosque · Enfoque Pomodoro (PWA)

Temporizador Pomodoro con un bosque que crece y se guarda entre sesiones, además de un **modo estricto** contra las distracciones.
Hecho con HTML, CSS y JavaScript, sin dependencias ni compilación. Funciona sin conexión y se puede instalar en iPhone, iPad y Mac.

## Archivos
| Archivo | Qué contiene |
|---|---|
| `index.html` | La app completa: estilos, ilustraciones SVG y lógica |
| `manifest.json` | El manifiesto web (modo standalone e íconos) |
| `sw.js` | El service worker que guarda la app en caché para usarla sin conexión |
| `icons/` | Los íconos (SVG y PNG de 180, 192 y 512 px) |

## Cómo publicarla
Sirve la carpeta `bosque/` por **HTTPS**, que es obligatorio para el service worker y el Wake Lock. Puedes usar GitHub Pages, Netlify, Cloudflare Pages o Vercel.
En local: `cd bosque && python3 -m http.server 8000` y abre `http://localhost:8000`.

**Instalar:** en Safari de iOS toca Compartir → *Agregar a pantalla de inicio*. En macOS (Safari 17 o posterior) usa Archivo → *Agregar al Dock*.

Cuando publiques cambios, sube la versión de `CACHE` en `sw.js` (por ejemplo `bosque-v2`) para que los dispositivos se actualicen.

## Datos y respaldo
- Todo se guarda en **IndexedDB**. Si no está disponible, se usa `localStorage`. La sesión en curso se guarda aparte de forma inmediata para resistir recargas o que iOS cierre la app.
- En **Ajustes ⚙︎ → Exportar respaldo** se genera un `.json`. En iPhone se abre la hoja de compartir, donde puedes elegir *Guardar en Archivos → iCloud Drive*.
- **Importar** te deja elegir entre *Fusionar* con lo que ya tienes o *Reemplazar todo*. El archivo se revisa antes de cargarlo.

## Modo estricto: cómo se comporta en iOS
- Se detecta la salida con `visibilitychange` (cambiar de app, bloquear la pantalla, cambiar de pestaña) y con `blur`/`focus` (otra ventana en macOS).
- En iOS Safari, JavaScript se pausa en segundo plano. Por eso el plazo de 20 s se calcula con la **hora real** en cuanto vuelves: si pasaron más de 20 s, el árbol se marchita aunque el temporizador interno no haya podido correr.
- `navigator.vibrate` no existe en iOS Safari. En su lugar hay una alarma con Web Audio, una pantalla roja y ámbar parpadeante y un destello de aviso al volver.
- El Wake Lock (iOS 16.4 o posterior) evita que la pantalla se bloquee sola. Si no está disponible, la app pide desactivar el bloqueo automático.
