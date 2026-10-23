# Historial de cambios — Laguna Media Lite

Todos los cambios notables de Laguna Media Lite se documentan en este archivo.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y
el proyecto usa [Versionado Semántico](https://semver.org/lang/es/).

---

## [1.0.4] — 2026-10-23

Funciona de fábrica: el motor actualizado viaja dentro de la app y la
actualización pasa a ser opcional, sin descargas automáticas ni acumulación.

### Corregido
- **El motor actualizado del usuario se ignoraba hasta reiniciar**: al arrancar
  se importaba el `yt-dlp` empaquetado antes de agregar la carpeta del usuario
  a `sys.path`; como Python guarda el módulo en `sys.modules`, la copia nueva
  no se usaba. Ahora la carpeta del usuario se activa antes de importar.
- **Cada pulsación de "Actualizar motor" volvía a descargar 3 MB** aunque ya
  estuvieras al día. Ahora se consulta la última release, se compara con la
  versión instalada y sólo se descarga si hay algo nuevo.
- **Una copia vieja del usuario tapaba el motor incluido** en Flatpak y
  AppImage: si en `~/.var/app/.../ytdlp` o `~/.local/share/lagunamedia/ytdlp`
  quedaba una versión anterior (por ejemplo `2026.08.19`), la app la usaba
  igual aunque el binario ya trajera `2026.10.21`. Ahora se comparan las dos
  versiones, se activa la más nueva y la copia obsoleta se elimina. Si la del
  usuario es más nueva, se respeta; si está dañada, se descarta y se usa la
  incluida.
- Se limpian `yt_dlp.old`, descargas temporales y cachés de consulta viejas; si
  una actualización se interrumpe, se recupera el motor anterior al arrancar.
- **Android**: el AAR de `youtubedl-android` traía yt-dlp `2025.11.12`, que ya
  no sirve para YouTube. El APK ahora incluye el zipapp oficial
  `2026.10.21` en `assets/` y lo copia antes de `YoutubeDL.init`. La app
  funciona desde la primera descarga sin pulsar "Actualizar motor".
- Las descargas de Android ya no actualizan el motor automáticamente ni
  reintentan a ciegas: si el motor falla, se enciende el aviso y el usuario
  decide cuándo actualizar.

### Agregado
- **Aviso opcional con punto rojo** en la navegación (escritorio) y en la
  tarjeta **Motor** (Android) cuando hay una versión nueva de yt-dlp. El
  chequeo de escritorio corre en segundo plano, con caché de 24 h, sin
  bloquear la ventana ni la descarga.
- **Android 3.0.4**: el mismo APK de la app completa, con el motor incluido y
  el aviso opcional. Se publica también en el Release de Lite.
- Script `android/scripts/fetch_ytdlp.py` y paso de CI que refrescan el
  `yt-dlp` incluido antes de compilar el APK.

### Verificado
- AppImage Lite 1.0.4 con HOME limpio: `--motor` informa `2026.10.21`,
  `--autodiagnostico` da OK y `--probar-descarga` descarga "Me at the zoo"
  sin crear ninguna copia del motor en el usuario.
- AppImage Lite con una copia vieja y rota (`2026.08.19`) en
  `~/.local/share/lagunamedia/ytdlp`: se detecta, se ignora, se borra y la
  descarga real sigue funcionando con el motor incluido. Con una copia
  `2099.01.01` sí se activa la del usuario.
- Flatpak Lite recompilado con la misma prueba: la copia real
  `2026.08.19` de `~/.var/app/com.lagunamedia.LagunaMediaLite/data/...` fue
  eliminada, `--motor` informa `2026.10.21`, `--autodiagnostico` da
  `RESULTADO: OK` y `--probar-descarga` descarga el audio de prueba.
- Android 3.0.4: instalación limpia en un Nokia 5.4 (Android 12) y en un
  Xiaomi 23117RA68G (Android 16). En ambos el APK copió el motor incluido
  `2026.10.21` antes de inicializar el SDK y la pantalla mostró esa versión,
  sin pulsar "Actualizar motor".
- Prueba instrumentada del aviso: al marcar una versión `2099.01.01` aparece
  el punto rojo, el texto de actualización y el botón "Actualizar a
  2099.01.01"; al limpiar el estado, desaparecen.
- Descarga real desde la UI del APK release en el Nokia 5.4 (Android 12):
  `Me at the zoo` en MP3 terminó en **Listo** con el motor incluido, sin
  pulsar "Actualizar motor".

---

## [1.0.3] — 2026-10-21

Primera publicación para Android.

### Agregado
- **`LagunaMedia-Android.apk`**: la app nativa para Android (Kotlin + Compose),
  con el mismo alcance que Lite (descargar de YouTube/TikTok y convertir, sin
  reproductor, biblioteca ni temas). Es el mismo APK que publica Laguna Media.
- El APK está **firmado con la clave de release** (antes se publicaba un build
  de debug) y soporta páginas de memoria de 16 KB, el requisito de los Android
  nuevos y de Google Play.

### Verificado
- Instalado y probado en un Nokia 5.4 (Android 12) y en un Xiaomi 23117RA68G
  (Android 16): descarga de YouTube completa, guardada en la galería, y
  validación del link de TikTok.

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
