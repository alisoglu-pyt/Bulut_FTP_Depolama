# Bulut Depolama

Заманауи, Material Design 3 Windows және Linux үшін шабыттандырылған **бір жақты FTP сақтық көшірмесін жасау** қолданбасы. Python + Tkinter көмегімен құрастырылған; сыртқы жұмыс уақытына тәуелділік жоқ.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Күй](docs/screenshots/en/01-dashboard.png)

| Қалталар | Кесте |
|---|---|
| ![Қалталар](docs/screenshots/en/02-folders.png) | ![Кесте](docs/screenshots/en/04-schedule.png) |
| FTP параметрлері | туралы |
| ![FTP Параметрлер](docs/screenshots/en/03-ftp.png) | ![Бағдарлама туралы](docs/screenshots/en/06-about.png) |

## Мүмкіндіктер

- **Бірнеше қалта** әр қалта үшін бөлек FTP тағайындау жолы бар.
- **Бір жақты синхрондау** жергілікті жерден серверге. Серверде ешнәрсе ешқашан жойылмайды; қосымша сервер файлдары сақталады.
- **Ақылды жүктеп салу**: өлшемі мен өзгерту уақыты сәйкес келген кезде файлдар өткізіп жіберіледі; тек жаңа және өзгертілген файлдар жүктеледі.
- **Жоспарлау**: сағат сайын (әр N сағат сайын), күн сайын, апта сайын (жұмыс күндерін таңдау), ай сайын. Келесі іске қосу уақыты UI ішінде көрсетіледі.
- **Құпия сөзді қорғау**: қолданба іске қосу кезінде құпия сөзді сұрайды. Параметрлер, FTP мәліметтері және қалталар тізімі онсыз жасырылады. Құпия сөзді өзгертуге және қолданбаны қолмен құлыптауға болады.
- **Шифрланған сақтау орны**: FTP құпия сөзі PBKDF2 (HMAC-SHA256 ағындық шифр + MAC) арқылы қолданба құпия сөзінен алынған кілтпен шифрланады және `config.json` ішінде сақталады.
- **Төзімділік**: автоматты қайта қосу және жалғастыру; жартылай жүктеп салулар бас тартылған кезде серверден тазартылады.
- **FTPS/TLS**, пассивті/белсенді режим, сертификатты тексеру, алып тастау үлгілері және жасырын файлды өңдеу.
- **17 тіл**, тікелей журнал, Windows жүйелік науасы және Windows/Linux автоқосылу.
- **Бет туралы** нұсқасы, әзірлеушісі және электрондық поштасы бар.

## Жылдам бастау

Python 3.10+ қажет (3.13 арқылы тексерілген).

```bash
python main.py
```

Бірінші іске қосқан кезде сіз **қолданба құпия сөзін** жасайсыз. Оны қалпына келтіру мүмкін емес; қолданбаны қалпына келтіру үшін `config.json` жойыңыз (барлық параметрлер мен сақталған FTP құпия сөзі жойылады).

Әдеттегі жұмыс процесі:

1. **FTP параметрлері** — хост, порт, пайдаланушы және құпия сөзді енгізіңіз; қажет болса FTPS қосу; сынақтан өткізіп, сақтаңыз.
2. **Қалталар** — бір немесе бірнеше жергілікті қалталарды қосыңыз. Жалғыз таңдау тағайындалған жолды сұрайды; бірнеше таңдаулар автоматты түрде `/FolderName` сәйкестендіріледі. Мақсаттарды кейінірек өңдеуге болады.
3. **Кесте** — жиілікті таңдап, оны қосыңыз және сақтаңыз. Жоспарлау тек қолданба ашық болған кезде орындалады.
4. **Күй** — қолмен іске қосу үшін «Қазір синхрондау» түймесін басыңыз және орындалу барысын, жылдамдығын, тікелей журналын және нәтижелердің қорытындысын бақылаңыз.
5. **About** — нұсқа, әзірлеуші ​​және байланыс деректері.

### Карталау мысалы

| Жергілікті | FTP тағайындалған орны |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` `/backup/projects/src/app.py` ретінде сақталады. Серверде бос қалталар жасалмайды.

## Деректер мен журнал орындары

- **Дереккөз режимі**: `data/config.json` және `logs/app.log` жобаның жанындағы.
- **Windows бумасы**: `%APPDATA%\BulutDepolama\data` және `%APPDATA%\BulutDepolama\logs`.
- **Linux бумасы**: `$XDG_CONFIG_HOME/bulut-depolama` (әдетте `~/.config/bulut-depolama`) `data` және `logs`.
- **Портативті режим**: `BULUT_PORTABLE=1` ортасының айнымалы мәні немесе орындалатын файлдың жанындағы `portable.flag` файлы.
- **Арнаулы орын**: `BULUT_DATA_DIR` ортасының айнымалысы.

## Құру

### Windows

Python 3.10+ және PyInstaller қажет (қажет болса сценарий оны орнатады).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Шығару: `dist\BulutDepolama.exe` (бір файлдық GUI). Белгіше `assets/icon.ico` ішінен келеді; `packaging/windows/version_info.txt` нұсқасының метадеректері.

### Linux

Ubuntu 22.04+ / Debian негізіндегі жүйелерде:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Шығарулар:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

AppImage іске қосыңыз (FUSE қажет):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

Сақтандырғышсыз:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb` орнату:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux жүйесінде жүйелік науа қолдауы жоқ; терезе әрекеті мен автоқосу XDG автоқосылу арқылы басқарылады.

## Файл қашан жүктеледі?

| Жағдай | Мінез-құлық |
|---|---|
| Серверде файл жоқ | Жүктелген (жаңа) |
| Өлшемі әртүрлі | Жүктелген (жаңартылған) |
| Жергілікті модификация уақыты жаңарақ (төзімділік: 120 с) | Жүктелген (жаңартылған) |
| Көлемі мен уақыты сәйкестігі | Өткізіп жіберді |
| Файл жергілікті түрде жойылды | Серверде сақталады |
| Сервердегі қосымша файл | Сақталған |

Күнді салыстыру серверде `MFMT` қолдауын қажет етеді; қол жетімді болмаса, өлшемді салыстыру пайдаланылады және бұл журналға жазылады.

## Тілдер

Интерфейс 17 тілді қолдайды:

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

Тілді кіру экранынан, тақырыптан немесе Қауіпсіздік бетінен өзгертіңіз. Аудармалар тікелей `app/locales/<code>.json`; жетіспейтін кілттер ағылшын тіліне қайта оралады.

## Тест

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` пакеттік активтерді және барлық 17 тіл файлдарын тексереді. Жоба E2E синхрондау, жою және жоспарлаушы әрекеті үшін жергілікті FTP серверімен (`pyftpdlib`) сыналған.

## Жоба құрылымы

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

## Қауіпсіздік жазбалары

- Қолданбаның құпия сөзі ешқашан дискіге жазылмайды; тек PBKDF2 тексерушісі сақталады (260 000 итерация, SHA-256).
- FTP құпия сөзі AES емес, стандартты кітапханалық HMAC-SHA256 ағындық шифрімен (шифрлау-сосын-MAC) шифрланған. Ол жергілікті файлдарға кіруден қорғайды; **FTPS** қосылмайынша, FTP трафигі шифрланбайды.
- Бес сәтсіз кіру әрекеті 15 секундтық кідірісті тудырады.

## Әзірлеуші

**Әзірлеуші:** Бекир Алишоғлу  
**Электрондық пошта:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Лицензия

**3 айлық демо лицензия** бірінші іске қосылғанда автоматты түрде жасалады. Барлық модульдер демонстрация кезінде жұмыс істейді. Оның мерзімі аяқталған кезде лицензия ескертуі көрсетіледі және толық лицензияны әкімші құпия сөзі бар Лицензияны басқару тақтасында екі күн арасында анықтауға болады.

- Лицензия файлы: `data/license.json`.
- Лицензия аппараттық құралға байланысты емес; файлды басқа компьютерге көшіруге болады.
- Жүйе сағатын артқа жылжыту лицензияны ұзартпайды; үлкен сағаттық өзгерістерден кейін лицензия соңғы сенімді күн арқылы бағаланады.

![Лицензия](docs/screenshots/en/07-license.png)