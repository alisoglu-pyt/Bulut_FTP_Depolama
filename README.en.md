# Bulut Depolama

Modern, Material Design 3 inspired **one-way FTP backup** application for Windows and Linux. Built with Python + Tkinter; no external runtime dependencies.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Status](docs/screenshots/en/01-dashboard.png)

| Folders | Schedule |
|---|---|
| ![Folders](docs/screenshots/en/02-folders.png) | ![Schedule](docs/screenshots/en/04-schedule.png) |
| FTP settings | About |
| ![FTP](docs/screenshots/en/03-ftp.png) | ![About](docs/screenshots/en/06-about.png) |

## Features

- **Multiple folders** with a separate FTP destination path for each folder.
- **One-way synchronization** from local to server. Nothing is ever deleted on the server; extra server files are preserved.
- **Smart upload**: files are skipped when size and modification time match; only new and changed files are uploaded.
- **Scheduling**: hourly (every N hours), daily, weekly (weekday selection), monthly. The next run time is shown in the UI.
- **Password protection**: the app asks for a password at startup. Settings, FTP details and the folder list are hidden without it. The password can be changed and the app can be locked manually.
- **Encrypted storage**: the FTP password is encrypted with a key derived from the app password using PBKDF2 (HMAC-SHA256 stream cipher + MAC) and stored in `config.json`.
- **Resilience**: automatic reconnect and resume; partial uploads are cleaned from the server when cancelled.
- **FTPS/TLS**, passive/active mode, certificate verification, exclusion patterns and hidden-file handling.
- **17 languages**, live log, Windows system tray and Windows/Linux autostart.
- **About page** with version, developer and e-mail.

## Quick start

Python 3.10+ is required (tested with 3.13).

```bash
python main.py
```

On first launch you create an **application password**. It cannot be recovered; delete `config.json` to reset the app (all settings and the saved FTP password are removed).

Typical workflow:

1. **FTP settings** — enter host, port, user and password; enable FTPS if needed; test and save.
2. **Folders** — add one or more local folders. A single selection asks for a destination path; multiple selections are mapped to `/FolderName` automatically. Destinations can be edited later.
3. **Schedule** — choose the frequency, enable it and save. Scheduling runs only while the app is open.
4. **Status** — click “Sync now” to run manually and follow progress, speed, live log and result summary.
5. **About** — version, developer and contact details.

### Mapping example

| Local | FTP destination |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` is stored as `/backup/projects/src/app.py`. Empty folders are not created on the server.

## Data and log locations

- **Source mode**: `data/config.json` and `logs/app.log` next to the project.
- **Windows package**: `%APPDATA%\BulutDepolama\data` and `%APPDATA%\BulutDepolama\logs`.
- **Linux package**: `$XDG_CONFIG_HOME/bulut-depolama` (usually `~/.config/bulut-depolama`) with `data` and `logs`.
- **Portable mode**: `BULUT_PORTABLE=1` environment variable or a `portable.flag` file next to the executable.
- **Custom location**: `BULUT_DATA_DIR` environment variable.

## Build

### Windows

Requires Python 3.10+ and PyInstaller (the script installs it if needed).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Output: `dist\BulutDepolama.exe` (single-file GUI). The icon comes from `assets/icon.ico`; version metadata from `packaging/windows/version_info.txt`.

### Linux

On Ubuntu 22.04+ / Debian-based systems:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Outputs:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

Run the AppImage (FUSE required):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

Without FUSE:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

Install the `.deb`:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux has no system-tray support; window behavior and autostart are managed through XDG autostart.

## When is a file uploaded?

| Situation | Behavior |
|---|---|
| File is missing on the server | Uploaded (new) |
| Size differs | Uploaded (updated) |
| Local modification time is newer (tolerance: 120 s) | Uploaded (updated) |
| Size and time match | Skipped |
| File was deleted locally | Kept on the server |
| Extra file on the server | Preserved |

Date comparison requires `MFMT` support on the server; if unavailable, size comparison is used and this is written to the log.

## Languages

The interface supports 17 languages:

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

Change the language from the login screen, the header or the Security page. Translations live in `app/locales/<code>.json`; missing keys fall back to English.

## Test

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` verifies the packaged assets, all 17 locale files and the license core. The project has been tested with a local FTP server (`pyftpdlib`) for E2E sync, cancellation and scheduler behavior.

## Project structure

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

## Security notes

- The app password is never written to disk; only the PBKDF2 verifier is stored (260,000 iterations, SHA-256).
- The FTP password is encrypted with a standard-library HMAC-SHA256 stream cipher (encrypt-then-MAC), not AES. It protects against local file access; FTP traffic itself is unencrypted unless you enable **FTPS**.
- Five failed login attempts trigger a 15-second delay.

## Developer

**Developer:** Bekir Alişoğlu  
**Email:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## License

A **3-month demo license** is created automatically on first run. All modules
work during the demo. When it expires, a license warning is shown and a full
license can be defined between two dates in the License Management panel with
the administrator password.

- License file: `data/license.json`.
- The license is not tied to hardware; the file can be copied to another computer.
- Moving the system clock back does not extend the license; after large clock
  changes the license is evaluated using the last trusted date.

![License management](docs/screenshots/en/07-license.png)