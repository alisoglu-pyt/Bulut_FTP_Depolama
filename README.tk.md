# Bulut Depolama

Döwrebap, Material Design 3 Windows we Linux üçin **bir taraplaýyn FTP ätiýaçlyk** programmasyny ylhamlandyrdy. Python + Tkinter bilen gurlan; daşarky iş wagtyna baglylyk ýok.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Ýagdaý](docs/screenshots/en/01-dashboard.png)

| Papkalar | Tertipnama |
| --- | --- |
| ![Bukjalar](docs/screenshots/en/02-folders.png) | ![Meýilnama](docs/screenshots/en/04-schedule.png) |
| FTP sazlamalary | Takmynan |
| ![FTP sazlamalary](docs/screenshots/en/03-ftp.png) | ![Hakynda](docs/screenshots/en/06-about.png) |

## Aýratynlyklar

- **Her bukja üçin aýratyn FTP niýetlenen ýoly bolan birnäçe bukja**.
- **localerli serwerden bir taraplaýyn sinhronizasiýa**. Serwerde hiç zat öçürilmeýär; goşmaça serwer faýllary saklanýar.
- **Akylly ýüklemek**: ululyk we üýtgetme wagty gabat gelende faýllar atylýar; diňe täze we üýtgedilen faýllar ýüklenýär.
- **Meýilnama**: sagatlyk (her N sagatda), gündelik, hepdelik (dynç güni saýlamak), aýlyk. Indiki iş wagty UI-de görkezilýär.
- **Paroly goramak**: programma başlangyçda parol soraýar. Sazlamalar, FTP jikme-jiklikleri we bukjanyň sanawy onsuz gizlenýär. Paroly üýtgedip we programma el bilen gulplap bolýar.
- **Şifrlenen ammar**: FTP paroly, PBKDF2 (HMAC-SHA256 akym şifrini + MAC) ulanyp, programma parolyndan alnan açar bilen kodlanýar we `config.json`-da saklanýar.
- **Çydamlylyk**: awtomatiki birikdirmek we dowam etdirmek; bölekleýin ýüklemeler ýatyrylanda serwerden arassalanýar.
- **FTPS / TLS**, passiw / işjeň re modeim, şahadatnamany barlamak, aýyrmak nagyşlary we gizlin faýl bilen işlemek.
- **17 dil**, janly gündelik, Windows ulgam tarelkasy we Windows / Linux awtostart.
- **Wersiýa, dörediji we e-poçta bilen** sahypa hakda.

## Çalt başlangyç

Python 3.10+ talap edilýär (3.13 bilen synag edilýär).

```bash
python main.py
```

Ilkinji işe girizilende ** programma parolyny döredýärsiňiz. Ony dikeldip bolmaz; programmany täzeden düzmek üçin `config.json` pozuň (ähli sazlamalar we saklanan FTP paroly aýrylýar).

Adaty iş tertibi:

1. **FTP sazlamalary** - host, port, ulanyjy we paroly giriziň; zerur bolsa FTPS-i işletmek; synap görüň we saklaň.
2. **Papkalar** - bir ýa-da birnäçe ýerli bukjany goşuň. Singleeke-täk saýlama barjak ýoluny soraýar; birnäçe saýlaw awtomatiki usulda `/FolderName` kartalaşdyrylýar. Niýetlenen ýerleri soň redaktirläp bolýar.
3. **Tertipnama** - ýygylygy saýlaň, işlediň we tygşytlaň. Meýilnama diňe programma açyk wagty işleýär.
4. ** ** agdaý ** - el bilen işlemek we ösüşi, tizligi, göni gündeligi we netijeleriň gysgaça mazmunyny yzarlamak üçin “Indi sinhronlamak” düwmesine basyň.
5. **Takmynan** - wersiýa, dörediji we aragatnaşyk maglumatlary.

### Kartalaşdyrmagyň mysaly

| | Erli | FTP barmaly ýeri |
| --- | --- |
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` `/backup/projects/src/app.py` görnüşinde saklanýar. Serwerde boş bukjalar döredilmeýär.

## Maglumatlar we gündelik ýerler

- **Çeşme tertibi**: `data/config.json` we taslamanyň gapdalynda `logs/app.log`.
- **Windows bukjasy**: `%APPDATA%\BulutDepolama\data` we `%APPDATA%\BulutDepolama\logs`.
- **Linux bukjasy**: `data` we `logs` bilen `$XDG_CONFIG_HOME/bulut-depolama` (köplenç `~/.config/bulut-depolama`).
- **Göçme re** im **: `BULUT_PORTABLE=1` gurşaw üýtgeýjisi ýa-da ýerine ýetirip boljak gapdalyndaky `portable.flag` faýly.
- **omörite ýer**: `BULUT_DATA_DIR` gurşaw üýtgeýjisi.

## Gur

### Windows

Python 3.10+ we PyInstaller talap edýär (skript zerur bolsa gurýar).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Çykyş: `dist\BulutDepolama.exe` (bir faýlly GUI). Nyşan `assets/icon.ico`; `packaging/windows/version_info.txt`-den wersiýa meta-maglumaty.

### Linux

Ubuntu 22.04+ / Debian esasly ulgamlarda:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Netijeler:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

“AppImage” -ni işlediň (FUSE zerur):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

FUSE bolmasa:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb` guruň:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux-da ulgamlaýyn goldaw ýok; penjiräniň özüni alyp barşy we awtostart XDG awtostart arkaly dolandyrylýar.

## Faýl haçan ýüklenýär?

| Uationagdaý | Özüňi alyp baryş |
| --- | --- |
| Serwerde faýl ýok | Uploadüklendi (täze) |
| Ölçegi tapawutlanýar | Uploadükledi (täzelendi) |
| Moderli üýtgetme wagty has täze (çydamlylyk: 120 s) | Uploadükledi (täzelendi) |
| Ölçeg we wagt deňeşdirmesi | Geçdi |
| Faýl ýerli derejede öçürildi | Serwerde saklandy |
| Serwerdäki goşmaça faýl | Saklandy |

Sene deňeşdirmek serwerde `MFMT` goldawyny talap edýär; elýeterli däl bolsa, ululygy deňeşdirmek ulanylýar we bu gündelige ýazylýar.

## Diller

Interfeýs 17 dili goldaýar:

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

Giriş ekranyndan, sözbaşydan ýa-da Howpsuzlyk sahypasyndan dili üýtgediň. Terjimeler `app/locales/<code>.json`; ýiten düwmeler iňlis diline gaýdyp gelýär.

## Synag

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` gaplanan aktiwleri we ähli 17 ýerli faýly barlaýar. Taslama ýerli FTP serweri (`pyftpdlib`) bilen E2E sinhronlamak, ýatyrmak we meýilleşdiriji özüni alyp barmak üçin synag edildi.

## Taslamanyň gurluşy

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

## Howpsuzlyk bellikleri

- Programmanyň paroly hiç haçan diske ýazylmaýar; diňe PBKDF2 barlaýjy saklanýar (260,000 gaýtalama, SHA-256).
- FTP paroly AES däl-de, adaty kitaphana HMAC-SHA256 akym şifri (şifrlenen-soň MAC) bilen kodlanýar. Fileerli faýl girişinden goraýar; **FTPS** açmasaňyz, FTP traffiginiň özi şifrlenen däldir.
- Giriş synanyşygynyň bäş sany synanyşygy 15 sekunt gijikdirýär.

## Öndüriji

**Öndüriji:** Bekir Alişoğlu  
**E-poçta:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Ygtyýarnama

Ilkinji işletmekde **3 aýlyk demo ygtyýarnama** awtomatiki döredilýär. Demo wagtynda ähli modullar işleýär. Möhleti gutaranda, ygtyýarnama duýduryşy görkezilýär we administratyryň paroly bilen Ygtyýarnama dolandyryş panelinde iki senäniň arasynda doly ygtyýarnama kesgitlenip bilner.

- Ygtyýarnama faýly: `data/license.json`.
- Ygtyýarnama enjam bilen baglanyşykly däl; faýly başga bir kompýutere göçürip bolýar.
- Ulgam sagadyny yza süýşürmek ygtyýarnamany uzaltmaýar; uly sagat üýtginlerinden soň, soňky ynamdar senäni ulanyp ygtyýarnama baha berilýär.

![Ygtyýarnama](docs/screenshots/en/07-license.png)