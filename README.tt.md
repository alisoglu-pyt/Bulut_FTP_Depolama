# Bulut Depolama

Заманча, Материаль Дизайн 3 Windows һәм Linux өчен **бер яклы FTP резерв** кушымтасын илһамландырды. Python + Tkinter белән төзелгән; тышкы эш вакыты бәйле түгел.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Статус](docs/screenshots/en/01-dashboard.png)

| Папкалар | Расписание |
| --- | --- |
| ![Папкалар](docs/screenshots/en/02-folders.png) | ![График](docs/screenshots/en/04-schedule.png) |
| FTP көйләүләре | Турында |
| ![FTP көйләүләр](docs/screenshots/en/03-ftp.png) | ![Программа туралы](docs/screenshots/en/06-about.png) |

## Функцияләр

- **Берничә папка** һәр папка өчен аерым FTP юнәлеш юлы белән.
- **Бер яклы синхронизация** җирледән серверга. Серверда бернәрсә дә бетерелми; өстәмә сервер файллары сакланган.
- **Акыллы йөкләү**: зурлык һәм модификация вакыты туры килгәндә файллар сикереп торалар; яңа һәм үзгәртелгән файллар гына йөкләнә.
- **Планлаштыру**: сәгать саен (һәр N сәгатьтә), көндәлек, атналык (ял көннәрен сайлау), ай саен. Киләсе эш вакыты UIда күрсәтелә.
- **Серсүзне саклау**: кушымта башланганда серсүз сорый. Көйләүләр, FTP детальләре һәм папка исемлеге ансыз яшерелгән. Серсүз үзгәртелергә һәм кушымта кул белән бикләнергә мөмкин.
- **Шифрланган саклагыч**: FTP серсүзе PBKDF2 (HMAC-SHA256 агым шифры + MAC) ярдәмендә кушымта серсүзеннән алынган ачкыч белән шифрланган һәм `config.json` сакланган.
- **Тынычлык**: автоматик яңадан тоташу һәм дәвам итү; өлешчә йөкләүләр сервердан чистартыла.
- **FTPS / TLS**, пассив / актив режим, сертификатны тикшерү, чыгару үрнәкләре һәм яшерен файл эшкәртү.
- **17 тел**, тере бүрәнә, Windows системасы подшипкасы һәм Windows / Linux автостарт.
- **Вариант, ясаучы һәм электрон почта белән** бит турында.

## Тиз башлау

Python 3.10+ кирәк (3.13 белән сынап карала).

```bash
python main.py
```

Беренче эшләтеп җибәргәндә сез ** кушымта серсүзен ясыйсыз. Аны торгызып булмый; кушымтаны яңадан урнаштыру өчен `config.json` бетерегез (барлык көйләүләр һәм сакланган FTP серсүзе бетерелә).

Типик эш процессы:

1. **FTP көйләүләре** - хуҗаны, портны, кулланучы һәм серсүзне кертегез; кирәк булса FTPSны эшләтеп җибәрү; сынап кара.
2. **Папкалар** - бер яки берничә җирле папка өстәргә. Бер сайлау юнәлеш юлын сорый; берничә сайлау автоматик рәвештә `/FolderName` белән ясалган. Максатлар соңрак редакцияләнергә мөмкин.
3. **Расписание** - ешлыкны сайлагыз, аны эшләгез һәм саклагыз. Планлаштыру кушымта ачык булганда гына эшли.
4. **Статус** - кул белән эшләп, алгарыш, тизлек, тере бүрәнә һәм нәтиҗәләр турында кыскача мәгълүмат ясау өчен "Хәзер синхронизация" төймәсенә басыгыз.
5. **Якынча** - версия, уйлап табучы һәм контакт мәгълүматлары.

### Карта ясау мисалы

| Localирле | FTP юнәлеше |
| --- | --- |
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` `/backup/projects/src/app.py` итеп саклана. Буш папкалар серверда ясалмый.

## Мәгълүматлар һәм бүрәнәләр урнашуы

- **Чыганак режимы**: `data/config.json` һәм `logs/app.log` проект янында.
- **Windows пакеты**: `%APPDATA%\BulutDepolama\data` һәм `%APPDATA%\BulutDepolama\logs`.
- **Linux пакеты**: `$XDG_CONFIG_HOME/bulut-depolama` (гадәттә `~/.config/bulut-depolama`) `data` һәм `logs` белән.
- **Көчле режим**: `BULUT_PORTABLE=1` мохит үзгәрүчесе яки башкарыла торган `portable.flag` файл.
- **Махсус урын**: `BULUT_DATA_DIR` мохит үзгәрүчесе.

## Төзегез

### Windows

Python 3.10+ һәм PyInstaller кирәк (сценарий аны кирәк булса урнаштыра).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Чыгыш: `dist\BulutDepolama.exe` (бер файллы GUI). Икона `assets/icon.ico`дан килә; `packaging/windows/version_info.txt` версиясе метадайыннары.

### Linux

Ubuntu 22.04+ / Дебиан системаларында:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Нәтиҗә:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

AppImage эшләгез (FUSE кирәк):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

FUSEсыз:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb` урнаштырыгыз:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux'ның система-подшипник ярдәме юк; тәрәзә тәртибе һәм автостарт XDG автостарт аша идарә ителә.

## Файл кайчан йөкләнә?

| Ситуация | Тәртип |
| --- | --- |
| Файл серверда юк | Йөкләнде (яңа) |
| Размеры аерыла | Йөкләнде (яңартылды) |
| Localирле үзгәртү вакыты яңарак (толерантлык: 120 с) | Йөкләнде (яңартылды) |
| Размер һәм вакыт матчы | Сикереп |
| Файл җирле итеп бетерелде | Серверда сакланган |
| Серверда өстәмә файл | Сакланган |

Дата чагыштыру серверда `MFMT` ярдәмен таләп итә; булмаса, зурлыкны чагыштыру кулланыла һәм бу бүрәнәгә языла.

## Телләр

Интерфейс 17 телдә ярдәм итә:

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

Логин экраныннан, башламнан яки Куркынычсызлык битеннән телне үзгәртегез. Тәрҗемәләр `app/locales/<code>.json`да яши; югалган ачкычлар инглиз теленә кире кайталар.

## Тест

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` пакетланган активларны һәм барлык 17 җирле файлны тикшерә. Проект E2E синхронизациясе, юкка чыгару һәм график тәртибе өчен җирле FTP серверы (`pyftpdlib`) белән сынады.

## Проект структурасы

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

## Куркынычсызлык язмалары

- Кушымта серсүзе беркайчан да дискка язылмый; бары тик PBKDF2 тикшерүче саклана (260,000 кабатлау, SHA-256).
- FTP серсүзе AES түгел, стандарт китапханә HMAC-SHA256 агым шифры (шифрланган-аннары MAC) белән шифрланган. Ул җирле файлларга керүдән саклый; **FTPS** кушмасагыз, FTP трафигының үзе шифрланмаган.
- Биш уңышсыз логин омтылышы 15 секундка соңга калырга этәрә.

## Төзүче

**Төзүче:** Бекир Алишоглу  
**Электрон почта:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Лицензия

** 3 айлык демо лицензия ** беренче тапкыр автоматик рәвештә ясала. Барлык модульләр дә демо вакытында эшли. Аның вакыты беткәч, лицензия турында кисәтү күрсәтелә һәм тулы лицензия Лицензия белән идарә итү панелендә администратор серсүзе белән ике дата арасында билгеләнергә мөмкин.

- Лицензия файлы: `data/license.json`.
- Лицензия җиһаз белән бәйләнмәгән; файлны башка санакка күчереп була.
- Система сәгатен артка күчерү лицензияне озайтмый; зур сәгать үзгәргәннән соң лицензия соңгы ышанычлы дата ярдәмендә бәяләнә.

![Лицензия](docs/screenshots/en/07-license.png)