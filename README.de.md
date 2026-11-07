# Bulut Depolama

Moderne, von Material Design 3 inspirierte **Einweg-FTP-Backup**-Anwendung für Windows und Linux. Gebaut mit Python + Tkinter; keine externen Laufzeitabhängigkeiten.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Status](docs/screenshots/en/01-dashboard.png)

| Ordner | Zeitplan |
|---|---|
| ![Ordner](docs/screenshots/en/02-folders.png) | ![Zeitplan](docs/screenshots/en/04-schedule.png) |
| FTP-Einstellungen | Über |
| ![FTP-Einstellungen](docs/screenshots/en/03-ftp.png) | ![Über](docs/screenshots/en/06-about.png) |

## Funktionen

- **Mehrere Ordner** mit einem separaten FTP-Zielpfad für jeden Ordner.
- **Einseitige Synchronisierung** von lokal zum Server. Auf dem Server wird nie etwas gelöscht; Zusätzliche Serverdateien bleiben erhalten.
- **Intelligenter Upload**: Dateien werden übersprungen, wenn Größe und Änderungszeit übereinstimmen; Es werden nur neue und geänderte Dateien hochgeladen.
- **Planung**: stündlich (alle N Stunden), täglich, wöchentlich (Auswahl an Wochentagen), monatlich. Die nächste Laufzeit wird in der Benutzeroberfläche angezeigt.
- **Passwortschutz**: Die App fragt beim Start nach einem Passwort. Einstellungen, FTP-Details und die Ordnerliste werden ohne diese Funktion ausgeblendet. Das Passwort kann geändert und die App manuell gesperrt werden.
- **Verschlüsselter Speicher**: Das FTP-Passwort wird mit einem aus dem App-Passwort abgeleiteten Schlüssel mithilfe von PBKDF2 (HMAC-SHA256-Stream-Verschlüsselung + MAC) verschlüsselt und in `config.json` gespeichert.
- **Belastbarkeit**: automatische Wiederverbindung und Fortsetzung; Teilweise Uploads werden beim Abbruch vom Server gelöscht.
- **FTPS/TLS**, Passiv-/Aktivmodus, Zertifikatsüberprüfung, Ausschlussmuster und Umgang mit versteckten Dateien.
- **17 Sprachen**, Live-Protokoll, Windows-Taskleiste und Windows/Linux-Autostart.
- **Infoseite** mit Version, Entwickler und E-Mail.

## Schnellstart

Python 3.10+ ist erforderlich (getestet mit 3.13).

```bash
python main.py
```

Beim ersten Start erstellen Sie ein **Anwendungspasswort**. Es kann nicht wiederhergestellt werden; Löschen Sie `config.json`, um die App zurückzusetzen (alle Einstellungen und das gespeicherte FTP-Passwort werden entfernt).

Typischer Arbeitsablauf:

1. **FTP-Einstellungen** – Host, Port, Benutzer und Passwort eingeben; Aktivieren Sie bei Bedarf FTPS. testen und speichern.
2. **Ordner** – Fügen Sie einen oder mehrere lokale Ordner hinzu. Eine einzelne Auswahl fragt nach einem Zielpfad. Mehrfachauswahlen werden automatisch `/FolderName` zugeordnet. Ziele können später bearbeitet werden.
3. **Zeitplan** – Häufigkeit auswählen, aktivieren und speichern. Die Planung wird nur ausgeführt, während die App geöffnet ist.
4. **Status** – Klicken Sie auf „Jetzt synchronisieren“, um den Vorgang manuell auszuführen und Fortschritt, Geschwindigkeit, Live-Protokoll und Ergebniszusammenfassung zu verfolgen.
5. **Über** – Version, Entwickler und Kontaktdaten.

### Mapping-Beispiel

| Lokal | FTP-Ziel |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` wird als `/backup/projects/src/app.py` gespeichert. Auf dem Server werden keine leeren Ordner erstellt.

## Daten- und Protokollspeicherorte

- **Quellmodus**: `data/config.json` und `logs/app.log` neben dem Projekt.
- **Windows-Paket**: `%APPDATA%\BulutDepolama\data` und `%APPDATA%\BulutDepolama\logs`.
- **Linux-Paket**: `$XDG_CONFIG_HOME/bulut-depolama` (normalerweise `~/.config/bulut-depolama`) mit `data` und `logs`.
- **Portabler Modus**: Umgebungsvariable `BULUT_PORTABLE=1` oder eine `portable.flag`-Datei neben der ausführbaren Datei.
- **Benutzerdefinierter Speicherort**: Umgebungsvariable `BULUT_DATA_DIR`.

## Bauen

### Windows

Erfordert Python 3.10+ und PyInstaller (das Skript installiert es bei Bedarf).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Ausgabe: `dist\BulutDepolama.exe` (Einzeldatei-GUI). Das Symbol stammt von `assets/icon.ico`; Versionsmetadaten von `packaging/windows/version_info.txt`.

### Linux

Auf Ubuntu 22.04+ / Debian-basierten Systemen:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Ausgänge:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

Führen Sie AppImage aus (FUSE erforderlich):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

Ohne SICHERUNG:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

Installieren Sie den `.deb`:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux bietet keine Taskleistenunterstützung. Fensterverhalten und Autostart werden über den XDG-Autostart verwaltet.

## Wann wird eine Datei hochgeladen?

| Situation | Verhalten |
|---|---|
| Datei fehlt auf dem Server | Hochgeladen (neu) |
| Größe variiert | Hochgeladen (aktualisiert) |
| Lokale Änderungszeit ist neuer (Toleranz: 120 s) | Hochgeladen (aktualisiert) |
| Größe und Zeit stimmen überein | Übersprungen |
| Datei wurde lokal gelöscht | Auf dem Server gespeichert |
| Zusätzliche Datei auf dem Server | Konserviert |

Der Datumsvergleich erfordert `MFMT`-Unterstützung auf dem Server. falls nicht verfügbar, wird ein Größenvergleich verwendet und dieser in das Protokoll geschrieben.

## Sprachen

Die Schnittstelle unterstützt 17 Sprachen:

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

Ändern Sie die Sprache über den Anmeldebildschirm, die Kopfzeile oder die Sicherheitsseite. Übersetzungen live in `app/locales/<code>.json`; fehlende Schlüssel fallen auf Englisch zurück.

## Test

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` überprüft die gepackten Assets und alle 17 Gebietsschemadateien. Das Projekt wurde mit einem lokalen FTP-Server (`pyftpdlib`) auf E2E-Synchronisierung, -Abbruch und -Planerverhalten getestet.

## Projektstruktur

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

## Sicherheitshinweise

- Das App-Passwort wird niemals auf die Festplatte geschrieben. Es wird nur der PBKDF2-Verifizierer gespeichert (260.000 Iterationen, SHA-256).
- Das FTP-Passwort wird mit einer HMAC-SHA256-Stream-Verschlüsselung der Standardbibliothek (Encrypt-then-MAC) verschlüsselt, nicht mit AES. Es schützt vor lokalen Dateizugriffen; Der FTP-Verkehr selbst ist unverschlüsselt, es sei denn, Sie aktivieren **FTPS**.
- Fünf fehlgeschlagene Anmeldeversuche lösen eine Verzögerung von 15 Sekunden aus.

## Entwickler

**Entwickler:** Bekir Alişoğlu  
**E-Mail:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Lizenz

Beim ersten Start wird automatisch eine **3-Monats-Demolizenz** erstellt. Während der Demo funktionieren alle Module. Bei Ablauf wird eine Lizenzwarnung angezeigt und im Lizenzverwaltungsbereich kann mit dem Administratorkennwort eine Volllizenz zwischen zwei Daten definiert werden.

- Lizenzdatei: `data/license.json`.
- Die Lizenz ist nicht an Hardware gebunden; die Datei kann auf einen anderen Computer kopiert werden.
- Durch das Zurückstellen der Systemuhr wird die Lizenz nicht verlängert; nach großen Zeitänderungen wird die Lizenz anhand des letzten vertrauenswürdigen Datums ausgewertet.

![Lizenz](docs/screenshots/en/07-license.png)