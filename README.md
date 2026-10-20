<p align="center">
  <img src="assets/logo.png" alt="Logo de Laguna Media Lite" width="180" />
</p>

<h1 align="center">Laguna Media Lite</h1>

<p align="center">
  <strong>Descargá de YouTube y TikTok, convertí formatos. Nada más. Rápido.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/Licencia-Propietaria-red.svg" alt="Licencia: Propietaria" /></a>
  <img src="https://img.shields.io/badge/Versión-1.0.1-blue.svg" alt="Versión 1.0.1" />
  <img src="https://img.shields.io/badge/Plataforma-Linux%20%7C%20Windows-lightgrey.svg" alt="Plataforma: Linux y Windows" />
  <img src="https://img.shields.io/badge/Tamaño-97--129%20MB-informational.svg" alt="Tamaño del binario" />
  <img src="https://img.shields.io/badge/Telemetría-ninguna-success.svg" alt="Sin telemetría" />
</p>

<p align="center">
  <a href="../../releases/latest"><img src="https://img.shields.io/badge/Descargar-última%20versión-yellow.svg" alt="Descargar" /></a>
</p>

Este repositorio distribuye **sólo los binarios compilados** de Laguna Media
Lite (AppImage y Flatpak para Linux, portable para Windows). El código fuente no
se publica.

---

## Qué es Laguna Media Lite

Laguna Media Lite es la versión mínima de **Laguna Media**: un proyecto
independiente, hecho para una sola cosa — **descargar y convertir lo más rápido
posible**. Arranca en menos de un segundo porque no carga reproductores,
biblioteca, miniaturas ni temas personalizables. Sólo tres secciones, en una
barra lateral.

| | Laguna Media | Laguna Media Lite |
|---|---|---|
| Descargar YouTube / TikTok | ✅ | ✅ |
| Convertir audio y video | ✅ | ✅ |
| Reproductor de música y video | ✅ | ❌ |
| Conversión de códec (H.264/HEVC/AV1) | ✅ | ❌ |
| Biblioteca con miniaturas y caché | ✅ | ❌ |
| Temas y personalización | ✅ | ❌ (tema fijo) |
| Secciones | 7 (pestañas) | **3 (barra lateral)** |
| Arranque | ~3 s | **< 1 s** |
| Portable para Windows | 112 MB | **97 MB** |
| AppImage para Linux | 163 MB | **129 MB** |
| Flatpak para Linux | ❌ | **107 MB** |

Si querés además reproductor, biblioteca y temas, la app completa está en
**[LagunaMedia-App](https://github.com/JonathanDevHurtado/LagunaMedia-App)**.

---

## Capturas de pantalla

| Descargar | Convertir |
|---|---|
| ![Sección Descargar](screenshots/01-descargar.png) | ![Sección Convertir](screenshots/02-convertir.png) |

<p align="center">
  <img src="screenshots/03-motor.png" alt="Sección Motor: actualización de yt-dlp" width="820" />
</p>

*Capturas de la app real, con rutas de ejemplo.*

---

## Descargas

Todos los binarios de la última versión están en
**[Releases → última versión](../../releases/latest)**.

| Archivo | Plataforma | Requisitos |
|---|---|---|
| [`LagunaMediaLite-x86_64.AppImage`](../../releases/latest/download/LagunaMediaLite-x86_64.AppImage) | Linux x86_64 | glibc 2.35+ (Fedora 36+, Ubuntu 22.04+, Debian 12+) + `libfuse2` |
| [`LagunaMediaLite-x86_64.flatpak`](../../releases/latest/download/LagunaMediaLite-x86_64.flatpak) | Linux x86_64 | Flatpak 1.10+ (Fedora, Ubuntu, Debian, etc.) |
| [`LagunaMediaLite-portable-win64.zip`](../../releases/latest/download/LagunaMediaLite-portable-win64.zip) | Windows 10 / 11 x64 | Ninguno (portable, sin instalador) |
| [`SHA256SUMS.txt`](../../releases/latest/download/SHA256SUMS.txt) | — | Checksums para verificar integridad |

**Verificar la descarga** (Linux):

```bash
sha256sum -c SHA256SUMS.txt
```

**Verificar la descarga** (Windows, PowerShell):

```powershell
Get-FileHash .\LagunaMediaLite-portable-win64.zip -Algorithm SHA256
# Comparar el resultado con la línea correspondiente de SHA256SUMS.txt
```

---

## Instalación

### Linux — Flatpak (recomendado en Fedora 41+)

Fedora y otras distribuciones nuevas ya no incluyen `libfuse2`, así que el
AppImage no abre ahí. El Flatpak funciona igual en todas:

```bash
flatpak install LagunaMediaLite-x86_64.flatpak
flatpak run com.lagunamedia.LagunaMediaLite
```

La primera vez, Flatpak pide instalar el runtime `org.freedesktop.Platform//24.08`
si no lo tenés (lo baja solo).

### Linux — AppImage

No hace falta instalar nada:

```bash
chmod +x LagunaMediaLite-x86_64.AppImage
./LagunaMediaLite-x86_64.AppImage
```

Si el sistema no tiene FUSE habilitado (o no tiene `libfuse2`):

```bash
./LagunaMediaLite-x86_64.AppImage --appimage-extract-and-run
```

En Fedora 41+ directamente no hay `libfuse2`: usá la versión Flatpak.

### Windows — portable

1. Descomprimí `LagunaMediaLite-portable-win64.zip`.
2. Ejecutá `lagunamedialite.exe` desde la carpeta descomprimida.

No hay instalador ni cambios en el registro. Para llevarla a otro lado, movés la
carpeta entera.

---

## Qué hace

### Descargar

Elegís la plataforma (**YouTube** o **TikTok**) y pegás el link en su propio
campo: cada una guarda su enlace y se valida por separado, así no hay que
adivinar de dónde viene. Después elegís:

- **Video con audio** — hasta 4K/2160p, según lo que ofrezca el video.
- **Solo audio** — MP3, Opus, M4A, FLAC o WAV, con bitrate seleccionable.


### Convertir

Elegís uno o varios archivos y los convertís a **MP3, Opus, M4A, FLAC, WAV,
OGG, MP4 (H.264) o MKV (H.264)**.

Los archivos se convierten **en paralelo**: varios a la vez, según los núcleos
de la CPU, en lugar de uno detrás de otro.

### Interfaz

Navegación lateral (sin pestañas), estética de bloques con bordes duros,
biseles y tipografía pixelada, sobre la paleta neon de Laguna. La ventana abre
directo: no hay pantalla de carga.

### Motor

Actualiza **yt-dlp** a la última versión oficial con un clic, sin reinstalar la
app. Es la solución al clásico *"YouTube ya no descarga"*: cuando el sitio
cambia algo, el motor se actualiza y todo vuelve a funcionar.

---

## Cómo está construida

Lite no es una versión "capada" de la app completa: es un proyecto aparte, que
comparte el mismo motor y el mismo contrato de nombres, pero no importa nada de
la app grande.

| Pieza | Tecnología |
|---|---|
| Interfaz | Python 3 + PySide6 (Qt Widgets), tema de bloques con tipografía pixelada |
| Descarga | yt-dlp (Unlicense) |
| Conversión de audio y video | FFmpeg estático (GPL) |
| Configuración | QSettings en `~/.config/LagunaMediaLite/` |
| Empaquetado | PyInstaller (onedir) + AppImage / Flatpak / ZIP portable |

### Descarga

- **8 fragmentos en paralelo** (`concurrent_fragment_downloads`), que es lo que
  más impacta en la velocidad de YouTube.
- Si **aria2c** está instalado, lo usa automáticamente (`-x 16 -s 16 -k 1M`).
- Progreso real de FFmpeg leído de `-progress pipe:1` (velocidad, tiempo
  restante y tamaño), no una barra inventada.
- Al extraer audio usa `-c:a copy` cuando el formato lo permite: no recomprime
  si no hace falta, así no se pierde calidad ni tiempo.

### Conversión

- **Cola paralela**: N conversiones simultáneas según los núcleos disponibles,
  con progreso individual por archivo.
- Cada trabajo corre en su propio hilo; la UI nunca se bloquea.
- Cancelación **cooperativa**: los workers se detienen solos y limpian los
  archivos temporales. Nunca se usa `QThread.terminate()` — matar un hilo que
  está ejecutando FFmpeg o yt-dlp corrompe el intérprete y termina en un
  segfault al cerrar la app.

### Actualización del motor

El actualizador baja la última versión oficial de yt-dlp desde GitHub Releases
y la deja en `~/.local/share/lagunamedia/ytdlp`. La app la carga desde ahí con
prioridad sobre la incluida en el binario. Así se puede arreglar el motor sin
publicar una versión nueva de la aplicación.

### Empaquetado

- **Linux**: PyInstaller `onedir` → AppDir → AppImage. La compilación se hace
  dentro de un contenedor Ubuntu 22.04 para que el binario corra en cualquier
  distribución con glibc 2.35 o superior.
- **Windows**: PyInstaller `onedir` → ZIP portable, sin instalador.
- **Flatpak**: manifiesto propio (`com.lagunamedia.LagunaMediaLite`) con
  `org.freedesktop.Platform//24.08`; PySide6-Essentials y yt-dlp se instalan
  dentro del sandbox y FFmpeg va como binario estático.
- El `.spec` de PyInstaller **excluye QtMultimedia** (Lite no reproduce nada):
  eso recorta decenas de MB del binario final.

---

## Decisiones de diseño

**¿Por qué existe Lite?** Porque mucha gente sólo quiere bajar un video o pasar
una canción a MP3, y no necesita un reproductor con biblioteca ni temas. Lite
abre al instante y ocupa menos.

**¿Por qué un proyecto separado y no un "modo simple"?** Porque cargar el
código del reproductor, la biblioteca y las miniaturas ya cuesta tiempo de
arranque y memoria, aunque no se use. Un proyecto chico, sin esas dependencias,
es más rápido y más simple de mantener.

**¿Por qué un tema fijo?** Personalizar colores y fondos implica cargar y
guardar configuración de estilo, escanear la carpeta de imágenes y repintar la
UI. Lite usa un único tema oscuro "neon" con estética de bloques (bordes duros,
biseles y tipografía pixelada) y se ahorra todo eso.

**¿Por qué el motor se actualiza en caliente?** YouTube cambia sus defensas cada
pocas semanas. Con el actualizador, un cambio no obliga a esperar una versión
nueva de la app.

**¿Por qué no se distribuye el código?** Es software propietario. Ver
[LICENSE](LICENSE).

---

## Requisitos

- **Linux**: x86_64. Con **Flatpak** funciona en cualquier distribución
  (Fedora, Ubuntu, Debian, Arch, etc.). El **AppImage** pide glibc 2.35 o
  superior (Fedora 36+, Ubuntu 22.04+, Debian 12+) y `libfuse2`.
- **Windows**: 10 u 11 de 64 bits. No requiere instalación.

---

## Control de calidad

- **Compilación reproducible en CI**: cada versión se compila desde cero en
  runners de Linux y Windows, no en la máquina del autor.
- **Pruebas de humo**: los binarios se ejecutan en el pipeline para verificar
  que abren y responden antes de publicarse.
- **Integridad**: cada release publica `SHA256SUMS.txt` con el hash SHA-256 de
  cada archivo. Cualquiera puede verificar que lo que descargó es exactamente
  lo que se publicó.
- **Sin dependencias ocultas**: la app no descarga nada al arrancar; el motor
  sólo se actualiza cuando el usuario lo pide.

---

## Privacidad

- **Sin telemetría, sin cuentas, sin analytics.** La app no manda datos a ningún
  servidor propio (no hay servidor propio).
- Sólo hace red cuando vos descargás algo (hacia YouTube/TikTok) o cuando pedís
  actualizar el motor (hacia GitHub Releases).
- La configuración y las descargas quedan en tu equipo.
- No incluye publicidad ni servicios de terceros.

---

## Solución de problemas

| Problema | Solución |
|---|---|
| YouTube "ya no descarga" | Sección **Motor** → *Actualizar motor de descargas* |
| El AppImage no abre en Fedora 41+ | Fedora ya no trae `libfuse2`: instalá la versión **Flatpak** |
| El AppImage no abre (FUSE) | `./LagunaMediaLite-x86_64.AppImage --appimage-extract-and-run` |
| El AppImage no abre (GLIBC) | Necesitás glibc 2.35+ (Fedora 36+, Ubuntu 22.04+, Debian 12+) |
| Descargas lentas | Instalá `aria2c` (`sudo dnf install aria2` / `sudo apt install aria2`) |
| Windows marca el `.exe` como desconocido | Es normal en binarios sin firma comercial: verificá el SHA-256 antes de ejecutar |

---

## Preguntas frecuentes

**¿Es legal descargar de YouTube o TikTok?**
La herramienta es neutra: sirve para descargar contenido propio, con permiso, o
material de dominio público. Vos sos responsable de respetar los términos de
servicio de cada plataforma y los derechos de autor. Ver el aviso legal al final.

**¿Puedo usarla sin conexión?**
Podés convertir archivos que ya tengas sin conexión. Descargar requiere
internet, obviamente.

**¿Funciona en macOS?**
No hay binario oficial para macOS.

**¿Dónde guarda las descargas?**
En la carpeta que elijas. Por defecto, en tu carpeta de Descargas.

**¿Necesito instalar FFmpeg o yt-dlp por separado?**
No: vienen incluidos en el binario.

**¿Por qué pesa 129 MB (AppImage) o 102 MB (Flatpak)?**
Porque incluye FFmpeg estático, Python y Qt. Es el costo de que funcione sin
dependencias en cualquier equipo.

**¿Habrá versión para Android?**
Lite es para escritorio. La app completa tiene versión Android.

---

## Cambios de la versión 1.0.1

- **Corregido**: la app se cerraba sola al abrir (Linux y Windows).
- **Nuevo**: versión **Flatpak**, que funciona en Fedora 41+ sin `libfuse2`.
- **Nuevo**: interfaz rediseñada con navegación lateral y estética de bloques.
- **Eliminado**: la pantalla de carga.
- **Eliminado**: la opción de cookies del navegador.
- **Nuevo**: YouTube y TikTok tienen selector y campo de link propios, con
  validación independiente.

---

## Proyecto relacionado

- **[Laguna Media](https://github.com/JonathanDevHurtado/LagunaMedia-App)** — la
  app completa: reproductor de música y video, biblioteca con miniaturas,
  conversión de códec, temas y versión Android.

---

## Licencia

**Propietaria — todos los derechos reservados.** Este repositorio distribuye
sólo binarios compilados; el código fuente no se publica ni se licencia.

Ver [LICENSE](LICENSE) para los términos completos y los avisos de los
componentes de terceros:

- **FFmpeg** — GPL (se distribuye como ejecutable separado, invocado por la app).
- **PySide6 / Qt** — LGPL v3.
- **yt-dlp** — Unlicense.

---

## Autor

**Jonathan Hurtado** — autor y mantenedor.

- GitHub: [@JonathanDevHurtado](https://github.com/JonathanDevHurtado)
- Correo: [JonathanHurtadDev@proton.me](mailto:JonathanHurtadDev@proton.me)

---

## Aviso legal

Laguna Media Lite es una herramienta técnica de descarga y conversión. **No está
afiliada, asociada, autorizada ni respaldada por YouTube, Google, TikTok ni
ByteDance.** Todas las marcas pertenecen a sus respectivos dueños.

El usuario es el único responsable del uso que le dé y de cumplir con las leyes
de su país, los términos de servicio de las plataformas y los derechos de autor
del contenido que descargue. El autor no se hace responsable por usos
indebidos.

