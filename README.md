<p align="center">
  <img src="assets/logo.png" alt="Logo de Laguna Media Lite" width="180" />
</p>

<h1 align="center">Laguna Media Lite</h1>

<p align="center">
  <strong>Descargá de YouTube y TikTok, convertí audio y video. Nada más. Sin distracciones.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/Licencia-Propietaria-red.svg" alt="Licencia: Propietaria" /></a>
  <img src="https://img.shields.io/badge/Versión-1.0.4-blue.svg" alt="Versión 1.0.4" />
  <img src="https://img.shields.io/badge/Plataforma-Linux%20%7C%20Windows-lightgrey.svg" alt="Plataforma: Linux y Windows" />
  <img src="https://img.shields.io/badge/Tamaño-102--153%20MB-informational.svg" alt="Tamaño del binario" />
  <img src="https://img.shields.io/badge/Telemetría-ninguna-success.svg" alt="Sin telemetría" />
</p>

<p align="center">
  <a href="../../releases/latest"><img src="https://img.shields.io/badge/⬇%20Descargar-última%20versión-yellow.svg" alt="Descargar la última versión" /></a>
  <a href="CHANGELOG.md"><img src="https://img.shields.io/badge/Historial-1.0.4-informational.svg" alt="Historial de cambios" /></a>
</p>

---

Este repositorio distribuye **sólo los binarios compilados** de Laguna Media
Lite para **Linux** (Flatpak y AppImage) y **Windows** (portable). El código
fuente no se publica: es un proyecto propietario.

Lite es la variante mínima de **Laguna Media**: hace dos cosas —descargar de
YouTube/TikTok y convertir formatos— y las hace rápido, sin reproductores, sin
biblioteca, sin temas y sin configuración.

---

## Tabla de contenido

- [Qué es Laguna Media Lite](#qué-es-laguna-media-lite)
- [Capturas de pantalla](#capturas-de-pantalla)
- [Descargas](#descargas)
- [Instalación](#instalación)
- [Primeros pasos](#primeros-pasos)
- [Qué hace](#qué-hace)
- [Requisitos](#requisitos)
- [Cómo está construida](#cómo-está-construida)
- [Solución de problemas](#solución-de-problemas)
- [Preguntas frecuentes](#preguntas-frecuentes)
- [Privacidad](#privacidad)
- [Calidad y verificación](#calidad-y-verificación)
- [Historial de cambios](#historial-de-cambios)
- [Proyecto relacionado](#proyecto-relacionado)
- [Licencia](#licencia)
- [Autor](#autor)
- [Aviso legal](#aviso-legal)

---

## Qué es Laguna Media Lite

| | Laguna Media | Laguna Media Lite |
|---|---|---|
| Descargar YouTube / TikTok | ✅ | ✅ |
| Convertir audio y video | ✅ | ✅ |
| Reproductor de música y video | ✅ | ❌ |
| Conversión de códec (H.264/HEVC/AV1) | ✅ | ❌ |
| Biblioteca con miniaturas y caché | ✅ | ❌ |
| Temas y personalización | ✅ | ❌ (tema fijo) |
| Secciones | 7 (pestañas) | **3 (barra lateral)** |
| Arranque | ~3 s | **casi instantáneo** |
| AppImage para Linux | 163 MB | **153 MB** |
| Flatpak para Linux | — | **107 MB** |
| Portable para Windows | 112 MB | **102 MB** |

Si además querés reproductor, biblioteca y temas, usá la app completa:
**[Laguna Media](https://github.com/JonathanDevHurtado/LagunaMedia-App)**. Para
Android, el APK es el mismo en los dos repositorios.

---

## Capturas de pantalla

| Descargar | Convertir |
|---|---|
| ![Sección Descargar](screenshots/01-descargar.png) | ![Sección Convertir](screenshots/02-convertir.png) |

<p align="center">
  <img src="screenshots/03-motor.png" alt="Sección Motor: actualización de yt-dlp" width="820" />
</p>

*Capturas de la aplicación real, con rutas de ejemplo.*

---

## Descargas

Todos los binarios de la última versión están en
**[Releases → última versión](../../releases/latest)**.

| Archivo | Plataforma | Requisitos |
|---|---|---|
| [`LagunaMediaLite-x86_64.flatpak`](../../releases/latest/download/LagunaMediaLite-x86_64.flatpak) | Linux x86_64 | Flatpak 1.10+ (Fedora, Ubuntu, Debian, Arch…) |
| [`LagunaMediaLite-x86_64.AppImage`](../../releases/latest/download/LagunaMediaLite-x86_64.AppImage) | Linux x86_64 | glibc 2.35+ (Fedora 36+, Ubuntu 22.04+, Debian 12+) y `libfuse2` |
| [`LagunaMediaLite-portable-win64.zip`](../../releases/latest/download/LagunaMediaLite-portable-win64.zip) | Windows 10 / 11 x64 | Ninguno (portable, sin instalador) |
| [`LagunaMedia-Android.apk`](../../releases/latest/download/LagunaMedia-Android.apk) | Android 7.0+ (API 24), cualquier arquitectura | ~212 MB de espacio libre |
| [`SHA256SUMS.txt`](../../releases/latest/download/SHA256SUMS.txt) | — | Huellas SHA-256 para verificar la descarga |

### Verificar la descarga (recomendado)

**Linux:**

```bash
sha256sum -c SHA256SUMS.txt
```

**Windows (PowerShell):**

```powershell
Get-FileHash .\LagunaMediaLite-portable-win64.zip -Algorithm SHA256
# Comparar el resultado con la línea correspondiente de SHA256SUMS.txt
```

---

## Instalación

### Linux — Flatpak (recomendado)

Funciona en cualquier distribución moderna, incluidas **Fedora 41+** y
**Ubuntu 24.04+**, donde el AppImage ya no abre porque no traen `libfuse2`.

```bash
flatpak install --user LagunaMediaLite-x86_64.flatpak
flatpak run com.lagunamedia.LagunaMediaLite
```

La primera vez, Flatpak descarga solo el runtime `org.freedesktop.Platform//24.08`
si no lo tenés instalado. La app queda en el menú de aplicaciones.

### Linux — AppImage

No hace falta instalar nada:

```bash
chmod +x LagunaMediaLite-x86_64.AppImage
./LagunaMediaLite-x86_64.AppImage
```

Si el sistema no tiene FUSE habilitado:

```bash
./LagunaMediaLite-x86_64.AppImage --appimage-extract-and-run
```

### Android — APK

1. Pasá `LagunaMedia-Android.apk` al teléfono (cable USB, nube o mensajería) y
   abrilo desde el explorador de archivos.
2. Cuando Android lo pida, permití **instalar apps de orígenes desconocidos**
   para esa app (explorador o navegador).
3. La primera vez, la app copia el motor yt-dlp incluido en el APK
   (`2026.10.21`) y prepara FFmpeg/aria2c: puede tardar unos segundos. Después
   arranca al instante.

Es el **mismo APK** que publica Laguna Media: la versión Android ya tiene el
alcance de Lite (descargar de YouTube/TikTok y convertir, sin reproductor,
biblioteca ni temas). Está firmado con la clave de release y soporta páginas
de memoria de 16 KB, el requisito de los Android nuevos.

```bash
adb install -r LagunaMedia-Android.apk
```

### Windows — portable

1. Descomprimí `LagunaMediaLite-portable-win64.zip` en cualquier carpeta.
2. Ejecutá `lagunamedialite.exe`.

No hay instalador, no toca el registro y no necesita permisos de
administrador. Para moverla, movés la carpeta completa.

---

## Primeros pasos

1. **Elegí dónde guardar.** La primera vez usa tu carpeta de Descargas; podés
   cambiarla desde la sección *Descargar*.
2. **Descargá algo.** Elegí **YouTube** o **TikTok**, pegá el link en el campo
   de esa plataforma y tocá **Descargar**. Podés bajar el video completo (hasta
   4K si el video lo ofrece) o sólo el audio.
3. **Convertí si hace falta.** En *Convertir* elegís uno o varios archivos y el
   formato de salida (MP3, Opus, M4A, FLAC, WAV, OGG, MP4 o MKV).
4. **Si hay un punto rojo en Motor**, podés aplicar la actualización opcional
   de yt-dlp. La app no la descarga ni la instala sola.

---

## Qué hace

### Descargar

- **YouTube y TikTok separados**: elegís la plataforma y cada una tiene su
  propio campo de link, con validación por dominio. No hay que adivinar de
  dónde viene la URL.
- **Video con audio** hasta 4K/2160p, según lo que ofrezca el video.
- **Sólo audio** en MP3, Opus, M4A, FLAC o WAV, con bitrate seleccionable.
- **8 fragmentos en paralelo** (`concurrent_fragment_downloads`) y uso
  automático de **aria2c** si está instalado.
- Progreso real: velocidad, tiempo restante y tamaño, leídos del propio
  descargador (no una barra inventada).

### Convertir

- Formatos de audio: **MP3, Opus, M4A, FLAC, WAV, OGG**.
- Formatos de video: **MP4 (H.264), MKV (H.264)**.
- Conversión **en paralelo**: varios archivos a la vez según los núcleos de la
  CPU, con progreso individual por archivo.
- `-c:a copy` cuando el formato lo permite: no recomprime el audio si no hace
  falta, así no se pierde calidad ni tiempo.

### Motor

- **El motor viene incluido de fábrica**: `yt-dlp 2026.10.21` y sus extractores
  de YouTube y TikTok viajan dentro del binario, así que la app descarga desde
  el primer arranque, incluso sin haber configurado nada.
- **Actualización opcional**: la app consulta como máximo una vez cada 24 h si
  hay una versión nueva. Si la hay, aparece un **punto rojo** en la sección
  *Motor*; no se descarga ni se instala nada hasta que lo pidas.
- **Sin acumulación**: la actualización se guarda en
  `~/.local/share/lagunamedia/ytdlp` (o `%LOCALAPPDATA%\LagunaMedia\ytdlp`).
  Al arrancar se compara con el motor incluido: gana la más nueva y la copia
  obsoleta se elimina. Si la del usuario está dañada, se descarta.
- La app sólo consulta GitHub para ver si hay una actualización; la descarga
  únicamente cuando aplicás el cambio.

---

## Requisitos

| Plataforma | Requisitos |
|---|---|
| **Linux (Flatpak)** | Flatpak 1.10 o superior. Cualquier distribución x86_64. |
| **Linux (AppImage)** | x86_64, glibc 2.35+ (Fedora 36+, Ubuntu 22.04+, Debian 12+) y `libfuse2`. |
| **Windows** | Windows 10 u 11 de 64 bits. Sin instalación. |

No hace falta instalar Python, FFmpeg ni yt-dlp: van incluidos en el binario.

---

## Cómo está construida

Lite no es una versión "capada" de la app completa: es un proyecto aparte que
comparte el mismo motor y el mismo contrato de nombres, pero no importa nada de
la app grande.

| Pieza | Tecnología |
|---|---|
| Interfaz | Python 3 + PySide6 (Qt Widgets) |
| Tema | QSS generado, estética de bloques con tipografías pixeladas (OFL) |
| Descarga | yt-dlp (Unlicense) |
| Conversión | FFmpeg estático (GPL) |
| Configuración | QSettings en `~/.config/LagunaMediaLite/` |
| Empaquetado | PyInstaller (onedir) → AppImage / Flatpak / ZIP portable |

**Por qué es liviana.** El archivo de PyInstaller excluye QtMultimedia y todo
el código de reproducción, biblioteca y miniaturas: son decenas de MB y
segundos de arranque que Lite no necesita.

**Por qué un proyecto separado y no un "modo simple".** Porque cargar el
reproductor, la biblioteca y las miniaturas cuesta tiempo y memoria aunque no
se usen. Un proyecto chico es más rápido y más simple de mantener.

**Por qué el motor se actualiza en caliente.** YouTube cambia sus defensas cada
pocas semanas. Con el actualizador, un cambio no obliga a esperar una versión
nueva de la app.

**Empaquetado.** Linux: PyInstaller `onedir` → AppDir → AppImage, compilado en
un contenedor Ubuntu 22.04 para que corra en cualquier distribución con glibc
2.35+. Windows: PyInstaller `onedir` → ZIP portable. Flatpak: manifiesto propio
con `org.freedesktop.Platform//24.08`, PySide6-Essentials y yt-dlp dentro del
sandbox y FFmpeg como binario estático.

---

## Solución de problemas

### Los tres casos más comunes

**1. El AppImage no abre en Fedora 41+ / Ubuntu 24.04+**

Esas distribuciones ya no incluyen `libfuse2`, que el AppImage necesita para
montarse. Dos salidas:

- Usá la versión **Flatpak** (recomendado), o
- ejecutá el AppImage sin FUSE:

  ```bash
  ./LagunaMediaLite-x86_64.AppImage --appimage-extract-and-run
  ```

**2. "YouTube ya no descarga" / error `unable to extract`**

YouTube cambió algo y el motor incluido quedó viejo. Si hay un **punto rojo**
en **Motor**, aplicá la actualización opcional: se baja la última versión
oficial de yt-dlp y la descarga vuelve a funcionar sin reinstalar la app.

**3. El link es rechazado**

El campo valida contra la plataforma elegida. Si pegaste un link de TikTok con
**YouTube** seleccionado, el mensaje lo dice: cambiá de plataforma o revisá la
URL. En TikTok, algunos videos viejos o restringidos por región devuelven
*"IP address is blocked from accessing this post"*: eso lo decide TikTok, no la
app; probá con otro video u otra red.

### Tabla rápida

| Síntoma | Causa | Solución |
|---|---|---|
| El AppImage no abre y no muestra nada | Falta `libfuse2` | Flatpak, o `--appimage-extract-and-run` |
| El AppImage muestra `GLIBC_2.xx not found` | Sistema más viejo que glibc 2.35 | Actualizá la distro o usá Flatpak |
| "YouTube no descarga" / `unable to extract` | Motor viejo o actualización disponible | Aplicá la actualización opcional si hay punto rojo |
| "Ese link no es de YouTube/TikTok" | Plataforma equivocada o URL incompleta | Cambiá la plataforma o copiá el link del video |
| TikTok: `IP address is blocked` | TikTok bloquea ese post por región o antigüedad | Probá con otro video u otra conexión |
| La descarga baja sin audio o sin video | El formato elegido no existe para ese video | Elegí *Mejor calidad disponible* o bajá el audio por separado |
| Descargas lentas | Una sola conexión | Instalá `aria2c` (`sudo dnf install aria2` / `sudo apt install aria2`) |
| Windows marca el `.exe` como desconocido | Binario sin firma comercial | Verificá el SHA-256 y ejecutá igual |
| El Flatpak no aparece en el menú | Instalación sólo para el usuario | `flatpak run com.lagunamedia.LagunaMediaLite` |
| La configuración no se guarda | Permisos de la carpeta de configuración | Revisá `~/.config/LagunaMediaLite/` |
| Error `CERTIFICATE_VERIFY_FAILED` al descargar (Linux) | Bug de 1.0.1: el binario compilado en Ubuntu no encontraba las CA en Fedora | Actualizá a **1.0.2** |
| La app se cerraba al escribir o pegar un link (Linux) | Bug de 1.0.1: el AppImage llevaba una `libxkbcommon` incompatible con la del sistema | Actualizá a **1.0.2** (o usá el Flatpak) |
| Quiero comprobar qué motor está usando | — | `./LagunaMediaLite-x86_64.AppImage --motor` |
| Quiero comprobar que el motor y ffmpeg están bien | — | `./LagunaMediaLite-x86_64.AppImage --autodiagnostico` |
| Quiero comprobar que descarga de verdad | — | `./LagunaMediaLite-x86_64.AppImage --probar-descarga` |

---

## Preguntas frecuentes

**¿Es legal descargar de YouTube o TikTok?**
La herramienta es neutra: sirve para descargar contenido propio, con permiso, o
material de dominio público. Vos sos responsable de respetar los términos de
servicio de cada plataforma y los derechos de autor.

**¿Necesito instalar FFmpeg, yt-dlp o Python?**
No. Todo va incluido en el binario.

**¿Dónde guarda las descargas?**
En la carpeta que elijas; por defecto, en tu carpeta de Descargas.

**¿Funciona sin conexión?**
Convertir archivos que ya tenés, sí. Descargar requiere internet.

**¿Funciona en macOS?**
No hay binario oficial para macOS.

**¿Por qué pesa 153 MB (AppImage) o 107 MB (Flatpak)?**
Porque incluye Python, Qt y FFmpeg estático. Es el costo de funcionar sin
dependencias en cualquier equipo.

**¿Hay versión Android de Lite?**
Sí. Se publica `LagunaMedia-Android.apk` en este repositorio: es la misma app
nativa que la completa y su alcance coincide con Lite (descargar y convertir,
sin reproductor, biblioteca ni temas). Está firmada con la clave de release y
soporta páginas de 16 KB.

**¿Por qué no publican el código?**
Es software propietario. Ver [LICENSE](LICENSE).

---

## Privacidad

- **Sin cuentas, sin telemetría, sin analítica, sin publicidad.** La app no
  tiene servidor propio y no manda datos a ningún lado.
- Sólo hace red en dos casos: cuando descargás algo (hacia YouTube/TikTok) y
  cuando consulta si hay una actualización del motor (hacia GitHub Releases,
  como máximo una vez cada 24 h). La descarga del motor sólo ocurre si la
  aplicás vos.
- La configuración y las descargas quedan en tu equipo.
- La comprobación de versiones no descarga ni instala nada sola: sólo enciende
  el punto rojo cuando hay una actualización opcional.

---

## Calidad y verificación

- **Compilación reproducible en CI**: cada versión se compila desde cero en
  runners de Linux (Ubuntu 22.04) y Windows, no en la máquina del autor.
- **Prueba de humo real**: en cada compilación, el CI ejecuta el binario de
  Linux **y** el de Windows para verificar dos cosas: que el motor viene
  incluido y el autodiagnóstico (`--motor`, `--autodiagnostico`: motor,
  extractores de YouTube/TikTok y ffmpeg). Si algo falla, el binario no se
  publica. Además se intenta una descarga real de audio (`--probar-descarga`);
  si YouTube bloquea la IP del runner, esa prueba es informativa y no invalida
  el binario.
- **Regresión de teclado**: el CI comprueba que el AppImage no vuelva a
  empaquetar `libxkbcommon` (el bug que cerraba la app al escribir en 1.0.1).
- **Integridad**: cada release publica `SHA256SUMS.txt` con el hash SHA-256 de
  cada archivo, para que puedas comprobar que lo que descargaste es exactamente
  lo que se publicó.

---

## Historial de cambios

Ver [CHANGELOG.md](CHANGELOG.md). Resumen de la versión actual:

- **1.0.4** — el motor `yt-dlp 2026.10.21` viaja incluido y la actualización
  pasa a ser opcional: punto rojo, sin descargas automáticas y sin acumular
  copias viejas. Incluye el APK 3.0.4, el mismo que Laguna Media.
- **1.0.3** — primera publicación para Android (`LagunaMedia-Android.apk`, el
  mismo APK que Laguna Media), firmada con la clave de release y compatible
  con páginas de 16 KB.
- **1.0.2** — arregla el cierre al escribir o pegar un link en Linux, refuerza
  el motor incluido (con reinstalación automática si fallara) y agrega
  diagnósticos `--motor` y `--probar-descarga`.
- **1.0.1** — arregla el cierre inesperado al abrir, agrega Flatpak, rediseño
  sin pestañas, sin pantalla de carga, sin cookies y con YouTube/TikTok
  separados.

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
- **Press Start 2P** y **VT323** — SIL Open Font License 1.1.

---

## Autor

**Jonathan Hurtado** — autor y mantenedor.

- GitHub: [@JonathanDevHurtado](https://github.com/JonathanDevHurtado)
- Correo: [JonathanHurtadoDev@proton.me](mailto:JonathanHurtadoDev@proton.me)

---

## Aviso legal

Laguna Media Lite es una herramienta técnica de descarga y conversión. **No está
afiliada, asociada, autorizada ni respaldada por YouTube, Google, TikTok ni
ByteDance.** Todas las marcas pertenecen a sus respectivos dueños.

El usuario es el único responsable del uso que le dé y de cumplir con las leyes
de su país, los términos de servicio de las plataformas y los derechos de autor
del contenido que descargue. El autor no se hace responsable por usos
indebidos.
