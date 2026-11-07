# Bulut Depolama

Müasir, Material Design 3 Windows və Linux üçün ilhamlanmış **birtərəfli FTP ehtiyat nüsxəsi** tətbiqi. Python + Tkinter ilə qurulmuşdur; xarici iş vaxtından asılılıq yoxdur.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Vəziyyət](docs/screenshots/en/01-dashboard.png)

| Qovluqlar | Cədvəl |
|---|---|
| ![Qovluqlar](docs/screenshots/en/02-folders.png) | ![Cədvəl](docs/screenshots/en/04-schedule.png) |
| FTP parametrləri | Haqqında |
| ![FTP parametrləri](docs/screenshots/en/03-ftp.png) | ![Haqqında](docs/screenshots/en/06-about.png) |

## Xüsusiyyətlər

- Hər qovluq üçün ayrıca FTP təyinat yolu ilə **Birdən çox qovluq**.
- Yerlidən serverə **Birtərəfli sinxronizasiya**. Serverdə heç nə silinmir; əlavə server faylları qorunur.
- **Ağıllı yükləmə**: ölçü və modifikasiya vaxtı uyğun gələndə fayllar atlanır; yalnız yeni və dəyişdirilmiş fayllar yüklənir.
- **Planlaşdırma**: saatlıq (hər N saatdan bir), gündəlik, həftəlik (həftə içi seçim), aylıq. Növbəti işləmə vaxtı UI-də göstərilir.
- **Parolun qorunması**: proqram başlanğıcda parol tələb edir. Parametrlər, FTP təfərrüatları və qovluq siyahısı onsuz gizlədilir. Parol dəyişdirilə bilər və proqram əl ilə kilidlənə bilər.
- **Şifrələnmiş yaddaş**: FTP parolu PBKDF2 (HMAC-SHA256 axın şifrəsi + MAC) istifadə edərək proqram parolundan əldə edilən açarla şifrələnir və `config.json`-da saxlanılır.
- **Dayanıqlıq**: avtomatik yenidən qoşulma və davam etdirmə; qismən yükləmələr ləğv edildikdə serverdən təmizlənir.
- **FTPS/TLS**, passiv/aktiv rejim, sertifikatın yoxlanılması, istisna nümunələri və gizli faylların idarə edilməsi.
- **17 dil**, canlı jurnal, Windows sistem tepsisi və Windows/Linux autostart.
- **Səhifə haqqında** versiya, tərtibatçı və e-poçt.

## Tez başlanğıc

Python 3.10+ tələb olunur (3.13 ilə sınaqdan keçirilib).

```bash
python main.py
```

İlk işə salındıqda siz **tətbiq parolunu** yaradırsınız. Onu bərpa etmək mümkün deyil; proqramı sıfırlamaq üçün `config.json`-nu silin (bütün parametrlər və saxlanmış FTP parolu silinir).

Tipik iş axını:

1. **FTP parametrləri** — host, port, istifadəçi və parol daxil edin; lazım olduqda FTPS-i aktivləşdirin; sınaqdan keçirin və yadda saxlayın.
2. **Qovluqlar** — bir və ya bir neçə yerli qovluq əlavə edin. Tək seçim təyinat yolunu soruşur; birdən çox seçim avtomatik olaraq `/FolderName` ilə əlaqələndirilir. Təyinatlar daha sonra redaktə edilə bilər.
3. **Cədvəl** — tezliyi seçin, onu aktivləşdirin və yadda saxlayın. Planlaşdırma yalnız proqram açıq olduqda işləyir.
4. **Status** — əl ilə işə salmaq və tərəqqi, sürət, canlı jurnal və nəticə xülasəsini izləmək üçün “İndi sinxronlaşdır” üzərinə klikləyin.
5. **Haqqında** — versiya, tərtibatçı və əlaqə məlumatları.

### Xəritəçəkmə nümunəsi

| Yerli | FTP təyinatı |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` `/backup/projects/src/app.py` kimi saxlanılır. Serverdə boş qovluqlar yaradılmır.

## Məlumat və qeyd yerləri

- **Mənbə rejimi**: `data/config.json` və `logs/app.log` layihənin yanında.
- **Windows paketi**: `%APPDATA%\BulutDepolama\data` və `%APPDATA%\BulutDepolama\logs`.
- **Linux paketi**: `$XDG_CONFIG_HOME/bulut-depolama` (adətən `~/.config/bulut-depolama`) `data` və `logs` ilə.
- **Portativ rejim**: `BULUT_PORTABLE=1` mühit dəyişəni və ya icra olunanın yanında `portable.flag` faylı.
- **Fərdi yer**: `BULUT_DATA_DIR` mühit dəyişəni.

## Qurmaq

### Windows

Python 3.10+ və PyInstaller tələb edir (lazım olduqda skript onu quraşdırır).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Çıxış: `dist\BulutDepolama.exe` (bir fayllı GUI). İşarə `assets/icon.ico`-dan gəlir; `packaging/windows/version_info.txt`-dan versiya metadatası.

### Linux

Ubuntu 22.04+ / Debian əsaslı sistemlərdə:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Çıxışlar:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

AppImage-i işə salın (FUSE tələb olunur):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

FUSE olmadan:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb` quraşdırın:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux-da sistem tepsisi dəstəyi yoxdur; pəncərə davranışı və autostart XDG autostart vasitəsilə idarə olunur.

## Fayl nə vaxt yüklənir?

| Vəziyyət | Davranış |
|---|---|
| Fayl serverdə yoxdur | Yükləndi (yeni) |
| Ölçü fərqlidir | Yükləndi (yeniləndi) |
| Yerli modifikasiya vaxtı daha yenidir (tolerantlıq: 120 s) | Yükləndi (yeniləndi) |
| Ölçü və vaxt uyğunluğu | Atlandı |
| Fayl yerli olaraq silindi | Serverdə saxlanılır |
| Serverdə əlavə fayl | Konservləşdirilmiş |

Tarixin müqayisəsi serverdə `MFMT` dəstəyini tələb edir; mövcud deyilsə, ölçü müqayisəsindən istifadə edilir və bu, jurnala yazılır.

## Dillər

İnterfeys 17 dili dəstəkləyir:

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

Giriş ekranından, başlıqdan və ya Təhlükəsizlik səhifəsindən dili dəyişdirin. Tərcümələr `app/locales/<code>.json`-da canlıdır; çatışmayan düymələr ingilis dilinə qayıdır.

## Test

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` paketlənmiş aktivləri və bütün 17 yerli faylları yoxlayır. Layihə E2E sinxronizasiyası, ləğvi və planlaşdırıcı davranışı üçün yerli FTP serveri (`pyftpdlib`) ilə sınaqdan keçirilmişdir.

## Layihə strukturu

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

## Təhlükəsizlik qeydləri

- Proqram parolu heç vaxt diskə yazılmır; yalnız PBKDF2 yoxlayıcısı saxlanılır (260.000 iterasiya, SHA-256).
- FTP parolu AES deyil, standart kitabxana HMAC-SHA256 axın şifrəsi (şifrlə-sonra-MAC) ilə şifrələnir. Yerli fayl girişindən qoruyur; **FTPS**-i aktiv etməyincə, FTP trafikinin özü şifrələnmir.
- Beş uğursuz giriş cəhdi 15 saniyəlik gecikməyə səbəb olur.

## Tərtibatçı

**Developer:** Bekir Alişoğlu  
**E-poçt:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Lisenziya

**3 aylıq demo lisenziyası** ilk işə salındıqda avtomatik olaraq yaradılır. Bütün modullar demo zamanı işləyir. Müddəti bitdikdə lisenziya xəbərdarlığı göstərilir və tam lisenziya administrator parolu ilə Lisenziya İdarəetmə panelində iki tarix arasında müəyyən edilə bilər.

- Lisenziya faylı: `data/license.json`.
- Lisenziya hardware ilə bağlı deyil; fayl başqa kompüterə kopyalana bilər.
- Sistem saatının geri çəkilməsi lisenziyanın müddətini uzatmır; böyük saat dəyişikliklərindən sonra lisenziya son etibarlı tarixdən istifadə etməklə qiymətləndirilir.

![Lisenziya](docs/screenshots/en/07-license.png)