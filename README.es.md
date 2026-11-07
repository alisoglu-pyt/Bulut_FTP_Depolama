# Bulut Depolama

Aplicación moderna de **copia de seguridad FTP unidireccional** inspirada en Material Design 3 para Windows y Linux. Construido con Python + Tkinter; sin dependencias de tiempo de ejecución externas.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Estado](docs/screenshots/en/01-dashboard.png)

| Carpetas | Horario |
|---|---|
| ![Carpetas](docs/screenshots/en/02-folders.png) | ![Programación](docs/screenshots/en/04-schedule.png) |
| Configuración de FTP | Acerca de |
| ![Configuración FTP](docs/screenshots/en/03-ftp.png) | ![Acerca de](docs/screenshots/en/06-about.png) |

## Características

- **Varias carpetas** con una ruta de destino FTP independiente para cada carpeta.
- **Sincronización unidireccional** del local al servidor. Nunca se elimina nada en el servidor; Se conservan los archivos adicionales del servidor.
- **Carga inteligente**: los archivos se omiten cuando el tamaño y la hora de modificación coinciden; sólo se cargan archivos nuevos y modificados.
- **Programación**: horaria (cada N horas), diaria, semanal (selección de días laborables), mensual. El siguiente tiempo de ejecución se muestra en la interfaz de usuario.
- **Protección con contraseña**: la aplicación solicita una contraseña al inicio. La configuración, los detalles de FTP y la lista de carpetas están ocultos sin él. La contraseña se puede cambiar y la aplicación se puede bloquear manualmente.
- **Almacenamiento cifrado**: la contraseña FTP se cifra con una clave derivada de la contraseña de la aplicación usando PBKDF2 (cifrado de flujo HMAC-SHA256 + MAC) y se almacena en `config.json`.
- **Resiliencia**: reconexión y reanudación automáticas; las cargas parciales se limpian del servidor cuando se cancelan.
- **FTPS/TLS**, modo pasivo/activo, verificación de certificados, patrones de exclusión y manejo de archivos ocultos.
- **17 idiomas**, registro en vivo, bandeja del sistema de Windows y inicio automático de Windows/Linux.
- **Página Acerca de** con versión, desarrollador y correo electrónico.

## Inicio rápido

Se requiere Python 3.10+ (probado con 3.13).

```bash
python main.py
```

En el primer inicio, crea una **contraseña de la aplicación**. No se puede recuperar; elimine `config.json` para restablecer la aplicación (se eliminan todas las configuraciones y la contraseña FTP guardada).

Flujo de trabajo típico:

1. **Configuración de FTP**: ingrese el host, el puerto, el usuario y la contraseña; habilite FTPS si es necesario; prueba y guarda.
2. **Carpetas**: agregue una o más carpetas locales. Una única selección solicita una ruta de destino; múltiples selecciones se asignan a `/FolderName` automáticamente. Los destinos se pueden editar más tarde.
3. **Programación**: elija la frecuencia, habilítela y guárdela. La programación se ejecuta solo mientras la aplicación está abierta.
4. **Estado**: haga clic en "Sincronizar ahora" para ejecutar manualmente y seguir el progreso, la velocidad, el registro en vivo y el resumen de resultados.
5. **Acerca de**: versión, desarrollador y detalles de contacto.

### Ejemplo de mapeo

| Locales | Destino FTP |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` se almacena como `/backup/projects/src/app.py`. Las carpetas vacías no se crean en el servidor.

## Ubicaciones de datos y registros

- **Modo fuente**: `data/config.json` y `logs/app.log` al lado del proyecto.
- **Paquete de Windows**: `%APPDATA%\BulutDepolama\data` y `%APPDATA%\BulutDepolama\logs`.
- **Paquete Linux**: `$XDG_CONFIG_HOME/bulut-depolama` (normalmente `~/.config/bulut-depolama`) con `data` y `logs`.
- **Modo portátil**: variable de entorno `BULUT_PORTABLE=1` o un archivo `portable.flag` al lado del ejecutable.
- **Ubicación personalizada**: variable de entorno `BULUT_DATA_DIR`.

## Construir

### Windows

Requiere Python 3.10+ y PyInstaller (el script lo instala si es necesario).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Salida: `dist\BulutDepolama.exe` (GUI de archivo único). El ícono proviene de `assets/icon.ico`; metadatos de la versión de `packaging/windows/version_info.txt`.

### Linux

En Ubuntu 22.04+/sistemas basados en Debian:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Salidas:

-`dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

Ejecute AppImage (se requiere FUSE):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

Sin FUSE:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

Instale el `.deb`:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux no tiene soporte para la bandeja del sistema; El comportamiento de la ventana y el inicio automático se gestionan a través del inicio automático de XDG.

## ¿Cuándo se carga un archivo?

| Situación | Comportamiento |
|---|---|
| Falta el archivo en el servidor | Subido (nuevo) |
| El tamaño difiere | Subido (actualizado) |
| La hora de modificación local es más reciente (tolerancia: 120 s) | Subido (actualizado) |
| Coincidencia de tamaño y tiempo | Saltado |
| El archivo fue eliminado localmente | Mantenido en el servidor |
| Archivo extra en el servidor | Conservado |

La comparación de fechas requiere compatibilidad con `MFMT` en el servidor; si no está disponible, se utiliza la comparación de tamaños y esto se escribe en el registro.

## Idiomas

La interfaz admite 17 idiomas:

- Türkçe
- English
- Deutsch
- Français
- Azərbaycanca
- Türkmençe
- Oʻzbekcha
- Português
- Español
- Русский
- Українська
- 日本語
- 简体中文
- 한국어
- Қазақша
- Татарча
- Тоҷикӣ

Cambie el idioma desde la pantalla de inicio de sesión, el encabezado o la página de Seguridad. Las traducciones se encuentran en vivo en `app/locales/<code>.json`; Las claves que faltan vuelven al inglés.

## Prueba

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` verifica los activos empaquetados y los 17 archivos locales. El proyecto ha sido probado con un servidor FTP local (`pyftpdlib`) para sincronización, cancelación y comportamiento del programador E2E.

## Estructura del proyecto

```
main.py                 Entry point (window, lock, event loop)
app/
  theme.py              Colors and typography
  widgets.py            Material components
  icons.py              Platform icons
  i18n.py               Language loading, tr(), date/weekday formatting
  locales/              17 language JSON files
  crypto.py             PBKDF2 key derivation, encryption/verification
  config.py             Settings file (atomic JSON)
  sync.py               FTP client and one-way sync engine
  scheduler.py          Hourly/daily/weekly/monthly scheduling
  context.py            Session, background jobs, event queue, logging
  folder_picker.py      Windows multi-folder picker (COM)
  tray.py               Windows system tray (disabled on Linux)
  startup.py            Windows registry / Linux XDG autostart
  system.py             Resource path and file-open helpers
  ui/                   Login, shell, language menu, pages
assets/                 icon.png, icon.ico, logo.png
packaging/windows/      PyInstaller build script and version info
packaging/linux/        AppImage / .deb build script, AppRun, .desktop
docs/screenshots/       Localized README screenshots
```

## Notas de seguridad

- La contraseña de la aplicación nunca se escribe en el disco; solo se almacena el verificador PBKDF2 (260.000 iteraciones, SHA-256).
- La contraseña FTP está cifrada con un cifrado de flujo HMAC-SHA256 de biblioteca estándar (cifrar y luego MAC), no con AES. Protege contra el acceso a archivos locales; El tráfico FTP en sí no está cifrado a menos que habilite **FTPS**.
- Cinco intentos fallidos de inicio de sesión provocan un retraso de 15 segundos.

## Desarrollador

**Desarrollador:** Bekir Alişoğlu  
**Correo electrónico:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Licencia

Una **licencia de demostración de 3 meses** se crea automáticamente en la primera ejecución. Todos los módulos funcionan durante la demostración. Cuando caduca, se muestra un aviso de licencia y se puede definir una licencia completa entre dos fechas en el panel de Gestión de licencias con la contraseña de administrador.

- Archivo de licencia: `data/license.json`.
- La licencia no está vinculada al hardware; el archivo se puede copiar a otra computadora.
- Retroceder el reloj del sistema no extiende la licencia; después de grandes cambios de reloj, la licencia se evalúa utilizando la última fecha confiable.

![Licencia](docs/screenshots/en/07-license.png)