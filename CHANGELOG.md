# Historial de cambios — Laguna Media Lite

Todos los cambios notables de Laguna Media Lite se documentan en este archivo.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y
el proyecto usa [Versionado Semántico](https://semver.org/lang/es/).

---

## [1.0.2] — 2026-10-20

Versión de corrección crítica para escritorio.

### Corregido
- **La app se cerraba al escribir o pegar un link en Linux.** El AppImage
  empaquetaba su propia copia de `libxkbcommon.so.0` mientras que el plugin
  `xcb` de Qt usaba la `libxkbcommon-x11` del sistema; esa mezcla provocaba
  `SIGSEGV` con la primera tecla. Ahora se usa la biblioteca del sistema y se
  puede escribir y pegar con normalidad.
- **Descargas con `CERTIFICATE_VERIFY_FAILED` en el AppImage publicado.** El
  binario se compila en Ubuntu y la ruta de certificados de OpenSSL no existe
  en Fedora, así que no se cargaba ninguna CA. Ahora el bundle de `certifi`
  viaja dentro del binario, independiente de la distribución.
- **Motor blindado**: si la copia incluida en el binario no se pudiera cargar
  (build incompleto o instalación dañada), la app descarga el motor oficial y
  reintenta sola antes de mostrar un error.

### Agregado
- `--motor`: imprime la versión de la app, el `yt-dlp` activo y la carpeta
  actualizable (también queda en `diagnostico.txt` en Windows).
- `--autodiagnostico`: verifica motor, extractores de YouTube/TikTok y ffmpeg
  sin usar la red. Es la prueba obligatoria del CI.
- `--probar-descarga`: descarga un audio corto y devuelve código 0/1. La usa
  el CI en Linux y Windows como prueba de humo real (informativa).
- El CI falla si un AppImage vuelve a empaquetar `libxkbcommon`.

---

## [1.0.1] — 2026-10-20

Versión de corrección y rediseño.

### Corregido
- **La aplicación se cerraba sola al abrir** (Linux y Windows). La ventana se
  creaba dentro del callback de un `QTimer` en una variable local; al terminar
  la función, Python la recolectaba y Qt cerraba la última ventana. Ahora la
  referencia vive durante todo el bucle de eventos.
- **El AppImage no abría en Fedora 41+ ni Ubuntu 24.04+**, donde ya no existe
  `libfuse2`. Se publica también como **Flatpak**, que no depende de FUSE.

### Agregado
- **Versión Flatpak** (`com.lagunamedia.LagunaMediaLite`): manifiesto, metadatos
  de tienda, lanzador e icono. Se distribuye como bundle en el Release.
- **Selector de plataforma** con un campo de link propio para YouTube y otro
  para TikTok, con validación por dominio.

### Cambiado
- **Interfaz nueva, sin pestañas**: barra lateral con tres secciones
  (Descargar, Convertir, Motor) y estética de bloques (biseles claros/oscuros,
  bordes duros, tipografía pixelada) sobre la paleta neón original.
- **Se eliminó la pantalla de carga**: la ventana abre directo.
- **Se eliminó la opción de cookies del navegador** de la interfaz, del motor y
  de la configuración. La app funciona sin configurar nada.

---

## [1.0.0] — 2026-10-17

Primera versión pública de **Laguna Media Lite**, la variante mínima y
optimizada para velocidad de Laguna Media. Proyecto separado: sólo descarga y
conversión, sin reproductor ni personalización.

### Agregado
- **Sección Descargar**: YouTube y TikTok, con detección automática de la
  plataforma. Modo *Video con audio* (hasta 2160p/4K) o *Solo audio*
  (MP3, Opus, M4A, FLAC, WAV con bitrate seleccionable).
- **Sección Convertir**: conversión por lotes a MP3, Opus, M4A, FLAC, WAV,
  OGG, MP4 (H.264) o MKV (H.264), **en paralelo** según los núcleos de CPU.
- **Sección Motor**: actualizador de **yt-dlp** en tiempo de ejecución, que
  baja la última versión oficial desde GitHub Releases sin reinstalar la app.
- **Botón Cancelar** en descargas y conversiones, con limpieza de temporales.
- **Tema fijo "neón"**, sin personalización, para arrancar más rápido.

### Rendimiento
- **Descarga multiparte**: 8 fragmentos en paralelo y uso automático de
  **aria2c** si está instalado.
- **Conversión en paralelo** con `ThreadPoolExecutor`.
- **Progreso real de FFmpeg** (`-progress pipe:1`) y `-c:a copy` cuando no hace
  falta recomprimir el audio.
- **Arranque casi instantáneo**: sin biblioteca, sin miniaturas y sin módulos
  Qt de multimedia en el binario.

---

## Notas

- El motor actualizable se comparte con Laguna Media: la carpeta
  `~/.local/share/lagunamedia/ytdlp` es la misma, así que actualizar el motor
  en una app también beneficia a la otra.
- La configuración propia vive en `~/.config/LagunaMediaLite/` (Linux) o en
  `HKEY_CURRENT_USER\Software\LagunaMediaLite\LagunaMediaLite` (Windows).
