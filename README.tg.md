# Bulut Depolama

Замони муосир, Material Design 3 илҳомбахши **захираи яктарафаи FTP** барои Windows ва Linux. Бо Python + Tkinter сохта шудааст; вобастагии берунии вақти корӣ нест.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Статус](docs/screenshots/en/01-dashboard.png)

| Папкаҳо | Ҷадвали |
|---|---|
| ![Папкаҳо](docs/screenshots/en/02-folders.png) | ![Ҷадвал](docs/screenshots/en/04-schedule.png) |
| Танзимоти FTP | Дар бораи |
| ![FTP Танзимот](docs/screenshots/en/03-ftp.png) | ![Дар бораи барнома](docs/screenshots/en/06-about.png) |

## Хусусиятҳо

- **Ҷузвдонҳои сершумор** бо роҳи алоҳидаи таъиноти FTP барои ҳар як ҷузвдон.
- **синхронизатсияи яктарафа** аз маҳаллӣ ба сервер. Ҳеҷ чиз дар сервер нест карда намешавад; файлҳои сервери иловагӣ нигоҳ дошта мешаванд.
- **Боркунии оқилона**: файлҳо ҳангоми мувофиқат кардани андоза ва вақти тағирот гузаронида мешаванд; танҳо файлҳои нав ва тағирёфта бор карда мешаванд.
- **Банақшагирӣ**: ҳар соат (ҳар N соат), ҳаррӯза, ҳарҳафтаина (интихоби рӯзи ҳафта), моҳона. Вақти навбатии кор дар UI нишон дода мешавад.
- **Ҳифзи парол**: барнома ҳангоми оғозёбӣ паролро мепурсад. Танзимотҳо, тафсилоти FTP ва рӯйхати ҷузвдонҳо бе он пинҳон мешаванд. Паролро иваз кардан мумкин аст ва барномаро дастӣ баста кардан мумкин аст.
- **Захираи рамзгузоришуда**: пароли FTP бо калиде, ки аз пароли барнома гирифта шудааст, бо истифода аз PBKDF2 (HMAC-SHA256 cipher stream + MAC) рамзгузорӣ шуда, дар `config.json` нигоҳ дошта мешавад.
- **Муқовимат**: пайвастшавӣ ба таври худкор ва идома; боргузорӣ қисман аз сервер тоза карда мешавад, вақте ки бекор.
- **FTPS/TLS**, ҳолати ғайрифаъол/фаъол, санҷиши сертификат, намунаҳои истисно ва коркарди файлҳои пинҳон.
- **17 забон**, сабти зинда, панели системаи Windows ва худкор оғоз кардани Windows/Linux.
- **Дар бораи саҳифа** бо версия, таҳиякунанда ва почтаи электронӣ.

## Оғози зуд

Python 3.10+ талаб карда мешавад (бо 3.13 санҷида шудааст).

```bash
python main.py
```

Ҳангоми оғози аввал шумо **пароли барнома** эҷод мекунед. Онро барқарор кардан мумкин нест; Барои аз нав танзимкунии барнома `config.json`-ро нест кунед (тамоми танзимот ва пароли FTP-и захирашуда хориҷ карда мешаванд).

Раванди кори маъмулӣ:

1. **Танзимоти FTP** — хост, порт, корбар ва паролро ворид кунед; агар лозим бошад, FTPS-ро фаъол созед; озмоиш ва захира кунед.
2. **Ҷузвдонҳо** — як ё якчанд ҷузвдонҳои маҳаллӣ илова кунед. Интихоби ягона роҳи таъинотро талаб мекунад; интихоби сершумор ба `/FolderName` ба таври худкор харита карда мешавад. Ҷойҳои таъинотро баъдтар таҳрир кардан мумкин аст.
3. **Ҷадвал** — басомадро интихоб кунед, онро фаъол созед ва захира кунед. Банақшагирӣ танҳо ҳангоми кушода будани барнома иҷро мешавад.
4. **Вазъ** — "Ҳозир синхронизатсия" -ро клик кунед, то дастӣ иҷро кунед ва пешрафт, суръат, сабти зинда ва хулосаи натиҷаҳоро пайгирӣ кунед.
5. **Дар бораи** — версия, таҳиягар ва тафсилоти тамос.

### Намунаи харитасозӣ

| маҳаллӣ | Макони таъиноти FTP |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` ҳамчун `/backup/projects/src/app.py` нигоҳ дошта мешавад. Дар сервер ҷузвдонҳои холӣ сохта намешаванд.

## Ҷойҳои маълумот ва сабт

- **Ҳолати манбаъ**: `data/config.json` ва `logs/app.log` дар паҳлӯи лоиҳа.
- **Бастаи Windows**: `%APPDATA%\BulutDepolama\data` ва `%APPDATA%\BulutDepolama\logs`.
- **Бастаи Linux**: `$XDG_CONFIG_HOME/bulut-depolama` (одатан `~/.config/bulut-depolama`) бо `data` ва `logs`.
- **Ҳолати портативӣ**: тағирёбандаи муҳити зисти `BULUT_PORTABLE=1` ё файли `portable.flag` дар паҳлӯи файли иҷрошаванда.
- **Макони фармоишӣ**: тағйирёбандаи муҳити зист `BULUT_DATA_DIR`.

## Сохтмон

### Windows

Python 3.10+ ва PyInstaller-ро талаб мекунад (агар лозим бошад, скрипт онро насб мекунад).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Натиҷа: `dist\BulutDepolama.exe` (GUI ягона файл). Нишона аз `assets/icon.ico` меояд; метамаълумоти версия аз `packaging/windows/version_info.txt`.

### Linux

Дар системаҳои Ubuntu 22.04+ / Debian:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Натиҷаҳо:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

AppImage-ро иҷро кунед (FUSE лозим аст):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

Бе FUSE:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb` -ро насб кунед:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux дастгирии табақи системавӣ надорад; рафтори равзана ва худкор оғоз тавассути XDG autostart идора карда мешавад.

## Файл кай бор карда мешавад?

| Вазъият | рафтор |
|---|---|
| Дар сервер файл нест | Боршуда (нав) |
| Андоза фарқ мекунад | Боргузорӣ (навсозӣ) |
| Вақти тағирдиҳии маҳаллӣ навтар аст (таҳаммулпазирӣ: 120 с) | Боргузорӣ (навсозӣ) |
| Андоза ва вақт мувофиқат | Гузаштан |
| Файл ба таври маҳаллӣ нест карда шуд | Дар сервер нигоҳ дошта мешавад |
| Файли иловагӣ дар сервер | Нигоҳдорӣ |

Муқоисаи сана дар сервер дастгирии `MFMT`-ро талаб мекунад; агар дастрас набошад, муқоисаи андоза истифода мешавад ва ин ба гузориш навишта мешавад.

## Забонҳо

Интерфейс 17 забонро дастгирӣ мекунад:

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

Забонро аз экрани воридшавӣ, сарлавҳа ё саҳифаи Амният иваз кунед. Тарҷумаҳо дар `app/locales/<code>.json` зиндагӣ мекунанд; калидҳои гумшуда ба забони англисӣ бармегарданд.

## Санҷиш

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` дороиҳои бастабандишуда ва ҳамаи 17 файлҳои маҳаллиро тафтиш мекунад. Лоиҳа бо сервери маҳаллии FTP (`pyftpdlib`) барои ҳамоҳангсозии E2E, бекоркунӣ ва рафтори нақшакаш санҷида шудааст.

## Сохтори лоиҳа

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

## Қайдҳои амниятӣ

- Пароли барнома ҳеҷ гоҳ ба диск навишта намешавад; танҳо санҷиши PBKDF2 нигоҳ дошта мешавад (260,000 такрор, SHA-256).
- Пароли FTP бо рамзгузории стандартии китобхонаи HMAC-SHA256 (рамзкунонӣ-пас-MAC) рамзгузорӣ шудааст, на AES. Он аз дастрасии файлҳои маҳаллӣ муҳофизат мекунад; Худи трафики FTP рамзнашуда аст, агар шумо **FTPS**-ро фаъол накунед.
- Панҷ кӯшиши нокомии воридшавӣ боиси таъхири 15 сония мегардад.

## Таҳиягар

**Таҳиягар:** Бекир Алишоғлу  
**Почтаи электронӣ:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Иҷозатнома

Литсензияи намоишии **3-моҳа** ба таври худкор ҳангоми бори аввал сохта мешавад. Ҳама модулҳо дар давоми намоиш кор мекунанд. Вақте ки мӯҳлати он ба охир мерасад, огоҳии иҷозатнома нишон дода мешавад ва литсензияи пурра метавонад дар байни ду сана дар панели идоракунии иҷозатнома бо пароли мудир муайян карда шавад.

- Файли иҷозатнома: `data/license.json`.
- Иҷозатнома ба сахтафзор алоқаманд нест; файлро ба компютери дигар нусхабардорӣ кардан мумкин аст.
- Гузаронидани соати системавӣ иҷозатномаро дароз намекунад; пас аз тағирёбии соати калон иҷозатнома бо истифода аз санаи охирини эътимоднок арзёбӣ мешавад.

![Иҷозатнома](docs/screenshots/en/07-license.png)