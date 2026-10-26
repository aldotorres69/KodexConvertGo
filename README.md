# KodexConvertGo 🚀

<p align="center">
  <b>El convertidor universal de archivos offline para tu escritorio.</b><br>
  Convierte más de 250 formatos arrastrando y soltando directamente en el cursor del ratón con un menú circular dinámico ("donut menu"), o mediante la línea de comandos. Sin subir nada a servidores externos: 100% privado y local.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Windows-10%20%7C%2011-3C3489" alt="Windows 10 | 11">
  <img src="https://img.shields.io/badge/Tecnolog%C3%ADa-C%23%20%2F%20Avalonia%20UI-534AB7" alt="Avalonia UI">
  <img src="https://img.shields.io/badge/Formatos-255%2B%20Soportados-1D9E75" alt="255+ Formatos">
  <img src="https://img.shields.io/badge/Privacidad-100%25%20Offline-6A62C9" alt="Offline">
  <a href="LICENSE"><img src="https://img.shields.io/badge/Licencia-MIT-brightgreen" alt="Licencia MIT"></a>
</p>

<p align="center">
  <a href="https://github.com/aldotorres69/KodexConvertGo/releases/download/v2.1.0/KodexConvertGo-2.1.0-win-Setup.exe">
    <img src="https://img.shields.io/badge/Descargar%20Instalador-.EXE%20(Windows)-2563eb?style=for-the-badge&logo=windows&logoColor=white" alt="Descargar Instalador">
  </a>
  &nbsp;&nbsp;
  <a href="https://github.com/aldotorres69/KodexConvertGo/releases/download/v2.1.0/KodexConvertGo-2.1.0-win-Portable.zip">
    <img src="https://img.shields.io/badge/Descargar%20Portable-.ZIP%20(Sin%20Instalar)-10b981?style=for-the-badge&logo=files&logoColor=white" alt="Descargar Portable">
  </a>
  &nbsp;&nbsp;
  <a href="https://github.com/aldotorres69/KodexConvertGo/releases/download/v2.1.0/Guia_de_Uso_KodexConvertGo.pdf">
    <img src="https://img.shields.io/badge/Manual%20de%20Uso-.PDF-dc2626?style=for-the-badge&logo=adobeacrobatreader&logoColor=white" alt="Manual de Uso PDF">
  </a>
</p>

---

## 🌟 Características Principales

<table>
  <tr>
    <td width="33%" valign="top">
      <b>🎯 Justo en tu cursor (Donut Menu)</b><br>
      Sin necesidad de abrir una ventana pesada. Mantén pulsada la tecla de activación (<kbd>Ctrl</kbd> en Windows / <kbd>Option</kbd> en macOS) y arrastra tus archivos: un menú radial flotante se desplegará instantáneamente en tu puntero mostrando únicamente los formatos de destino compatibles.
    </td>
    <td width="33%" valign="top">
      <b>📦 +250 Formatos sin software adicional</b><br>
      Imágenes (incluyendo HEIC y RAW), documentos (PDF, Word, Excel, PowerPoint), audio, vídeo, libros electrónicos, archivos comprimidos, fuentes tipográficas, planos CAD y modelos 3D sin depender de Microsoft Office instalado.
    </td>
    <td width="33%" valign="top">
      <b>🔒 100% Privado y Offline</b><br>
      Tus datos nunca salen de tu ordenador. Todo el procesamiento y las transformaciones se ejecutan localmente en tu CPU/GPU, protegiendo al 100% tu información confidencial.
    </td>
  </tr>
</table>

---

## 📂 Formatos Compatibles (Resumen de Categorías)

- **Imágenes Raster y RAW**: PNG, JPG, WebP, AVIF, HEIC, TIFF, BMP, GIF, ICO, PSD, PSB, SVG, OpenEXR, RAW de cámaras (CR2, NEF, ARW, DNG, etc.).
- **Documentos & Ofimática**: PDF, Word (DOCX, DOC), Excel (XLSX, XLS), PowerPoint (PPTX, PPT), OpenDocument (ODT, ODS, ODP), RTF, Markdown, TXT.
- **Vídeo & Audio**: MP4, MKV, WebM, MOV, AVI, MP3, WAV, FLAC, AAC, OGG, Opus, M4A, GIF animado (conversión, recorte y unión con aceleración por hardware).
- **Libros Electrónicos**: EPUB, MOBI, AZW3, FB2, CBZ, CBR.
- **Archivos Comprimidos**: ZIP, 7Z, TAR, GZ, BZ2, XZ, RAR (extracción e inspección).
- **Fuentes Tipográficas**: TTF, OTF, WOFF, WOFF2, EOT.
- **Modelos 3D & CAD**: STL, OBJ, GLTF, GLB, 3MF, PLY, DXF, DWG.
- **Datos & Geoespaciales**: JSON, CSV, TSV, XML, YAML, TOML, GeoJSON, KML, Shapefile, GPX.

---

## 🛠️ ¿Cómo Funciona la Arquitectura?

```
            ┌──────────────────────────── KodexConvertGo.App (Avalonia UI) ────────────────┐
 global     │ TriggerService ──► GestureDetector ──► RadialController ──► RadialWindow      │
 mouse/keys │   (SharpHook)        (Máquina de          (Modo arrastre/     (Ventana        │
 ─────────► │                       estados)             clic)               transparente)  │
            │                                              │ drop / clic                    │
 Explorador │ SingleInstance (Named Pipe) ─────────────────►│                               │
 Contextual │ Drop Zone en ventana principal ──────────────┘                                │
            │                                              ▼                                │
            │                                    ConversionService ──► ToastWindow          │
            └──────────────────────────────────────────────┬────────────────────────────────┘
                                                           ▼
            ┌──────────────────────────── KodexConvertGo.Core ─────────────────────────────┐
            │ JobQueue ──► RoutePlanner (Algoritmo Dijkstra sobre grafos de conversión)     │
            │ FormatRegistry · Presets · OutputPathResolver · ProfileSelector               │
            └──────────────────────────────────────────────┬────────────────────────────────┘
                                                           ▼
            ┌──────────────────────────── KodexConvertGo.Engines ──────────────────────────┐
            │ Nativos integrados: SkiaSharp, Magick.NET, PdfSharp, Pdfium, SharpCompress    │
            │ Envoltorios de soporte: FFmpeg, Pandoc, LibreOffice, Calibre, Ghostscript    │
            └──────────────────────────────────────────────────────────────────────────────┘
```

1. **Detección Global (`TriggerService`)**:
   Utiliza `SharpHook` para interceptar eventos de entrada del mouse/teclado a nivel de sistema operativo sin bloquear la interfaz. Cuando detecta que el usuario arrastra un archivo manteniendo presionada la tecla modificadora, abre la ventana `RadialWindow`.
2. **Ventana Flotante No Intrusiva (`RadialWindow`)**:
   Es una ventana Avalonia borderless, totalmente transparente y marcada con `Topmost=True` y `ShowActivated=False`. Esto permite recibir el evento de arrastre (`DragEnter` / `Drop`) sin robarle el foco al Explorador de Windows o Finder.
3. **Planificación de Rutas (`RoutePlanner`)**:
   Implementa un algoritmo de grafos tipo **Dijkstra** que calcula la ruta de menor costo de transformación entre el formato origen y destino (hasta 3 saltos intermedios si una conversión directa no existe).
4. **Ejecución Local Concurrente (`JobQueue`)**:
   Gestiona la cola de trabajo aprovechando los hilos de CPU disponibles con límites de concurrencia adaptados para mantener la fluidez del equipo.

---

## 💻 Uso por Línea de Comandos (CLI)

KodexConvertGo incluye una interfaz de terminal (`kodexconvertgo` / `filee`):

```bash
# Convertir una imagen a WebP con calidad específica
kodexconvertgo convert foto.png --to webp --quality 85

# Convertir por lotes en una carpeta destino
kodexconvertgo convert *.heic --to jpg -o ./imagenes_convertidas

# Supervisar una carpeta y convertir automáticamente los archivos entrantes
kodexconvertgo watch D:\Descargas --to pdf --move-originals
```

---

## 📥 Descarga e Instalación Fácil

Elige la opción que mejor se adapte a tus necesidades para Windows:

| Opción | Archivo | Tamaño | ¿Cuándo elegirla? |
| :--- | :--- | :---: | :--- |
| **💻 Instalador Completo (.exe)** | [**Descargar KodexConvertGo-Setup.exe**](https://github.com/aldotorres69/KodexConvertGo/releases/download/v2.1.0/KodexConvertGo-2.1.0-win-Setup.exe) | ~112 MB | **Recomendada**. Instala la aplicación en tu sistema, crea accesos directos y habilita la integración con el Explorador de Windows. |
| **📦 Versión Portable (.zip)** | [**Descargar KodexConvertGo-Portable.zip**](https://github.com/aldotorres69/KodexConvertGo/releases/download/v2.1.0/KodexConvertGo-2.1.0-win-Portable.zip) | ~120 MB | **Sin instalación**. Ideal para llevar en una memoria USB o usar sin permisos de administrador. Solo descomprime y ejecuta `Filee.App.exe`. |
| **📖 Guía Visual de Usuario (PDF)** | [**Descargar Guía en PDF**](https://github.com/aldotorres69/KodexConvertGo/releases/download/v2.1.0/Guia_de_Uso_KodexConvertGo.pdf) | ~660 KB | Manual paso a paso con capturas explicativas sobre cómo usar el Menú Donut y configurar conversiones. |

### 🚀 Pasos para empezar a usarla:

1. **Si elegiste el Instalador (.exe):**
   * Descarga el archivo `KodexConvertGo-2.1.0-win-Setup.exe`.
   * Haz doble clic sobre el archivo descargado y sigue los pasos del asistente de instalación.
   * ¡Listo! KodexConvertGo se iniciará automáticamente y quedará activa en la bandeja del sistema (junto al reloj de Windows).

2. **Si elegiste la versión Portable (.zip):**
   * Descarga `KodexConvertGo-2.1.0-win-Portable.zip`.
   * Haz clic derecho sobre el archivo descargado y selecciona **Extraer todo...**.
   * Abre la carpeta extraída y haz doble clic en `Filee.App.exe` (puedes crearle un acceso directo en tu escritorio para abrirlo cómodamente).

3. **Cómo convertir tu primer archivo en 3 segundos:**
   * Abre cualquier carpeta en tu PC.
   * Selecciona un archivo (imagen, PDF, Word, video, etc.).
   * Mantén presionada la tecla <kbd>Ctrl</kbd> y arrastra el archivo con el mouse.
   * Aparecerá el menú circular (**Donut Menu**) directamente en el cursor: suelta el archivo sobre el formato que desees y ¡listo! Tu archivo convertido se guardará inmediatamente.

---

## 🔨 Compilación y Desarrollo

### Requisitos
- **Windows 10 / 11** (x64 o ARM64), macOS o Linux.
- **.NET SDK** (versión 10.0 o 9.0).

### Pasos para compilar:
```bash
# Clonar el repositorio
git clone https://github.com/aldotorres69/KodexConvertGo.git
cd KodexConvertGo

# Ejecutar la aplicación en modo desarrollo
dotnet run --project src/Filee.App
```

---

## 📄 Licencia y Atribución

Este proyecto está licenciado bajo la **Licencia MIT**.
KodexConvertGo es un desarrollo derivado basado en el proyecto de código abierto **Filee**, creado y publicado originalmente por **Myeonggu Kim (KnifeLemon)** y sus colaboradores.

Todos los avisos de derechos de autor y licencias de terceros se preservan íntegramente en [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md) y [LICENSE](LICENSE).
