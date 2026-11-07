# Bulut Depolama

Zamonaviy, Material Design 3 Windows va Linux uchun ilhomlantirilgan **bir tomonlama FTP zaxiralash** ilovasi. Python + Tkinter bilan yaratilgan; tashqi ish vaqtiga bog'liqlik yo'q.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Holat](docs/screenshots/en/01-dashboard.png)

| Jildlar | Jadval |
|---|---|
| ![Jildlar](docs/screenshots/en/02-folders.png) | ![Jadval](docs/screenshots/en/04-schedule.png) |
| FTP sozlamalari | Haqida |
| ![FTP sozlamalari](docs/screenshots/en/03-ftp.png) | ![Haqida](docs/screenshots/en/06-about.png) |

## Xususiyatlar

- **Bir nechta jildlar**, har bir jild uchun alohida FTP manzil yo'li.
- **Bir tomonlama sinxronizatsiya** mahalliydan serverga. Serverda hech narsa o'chirilmaydi; qo'shimcha server fayllari saqlanadi.
- **Aqlli yuklash**: hajmi va oʻzgartirish vaqti mos kelganda fayllar oʻtkazib yuboriladi; faqat yangi va o'zgartirilgan fayllar yuklanadi.
- **Rejalashtirish**: soatlik (har N soatda), kunlik, haftalik (hafta kunini tanlash), oylik. Keyingi ish vaqti UIda ko'rsatilgan.
- **Parol himoyasi**: dastur ishga tushganda parol so‘raydi. Sozlamalar, FTP tafsilotlari va papkalar ro'yxati ularsiz yashiringan. Parolni o'zgartirish va ilovani qo'lda qulflash mumkin.
- **Shifrlangan xotira**: FTP paroli PBKDF2 (HMAC-SHA256 oqim shifr + MAC) yordamida ilova parolidan olingan kalit bilan shifrlanadi va `config.json` da saqlanadi.
- **Mustahkamlik**: avtomatik qayta ulanish va davom ettirish; qisman yuklashlar bekor qilinganda serverdan tozalanadi.
- **FTPS/TLS**, passiv/faol rejim, sertifikatni tekshirish, istisno qilish naqshlari va yashirin fayllar bilan ishlash.
- **17 til**, jonli jurnal, Windows tizim tepsisi va Windows/Linux autostart.
- **Haqida sahifa** versiya, ishlab chiquvchi va elektron pochta bilan.

## Tez boshlash

Python 3.10+ talab qilinadi (3.13 bilan sinovdan o'tgan).

```bash
python main.py
```

Birinchi ishga tushirishda siz **ilova parolini** yaratasiz. Uni qayta tiklash mumkin emas; ilovani asl holatiga qaytarish uchun `config.json` ni o‘chirib tashlang (barcha sozlamalar va saqlangan FTP paroli o‘chiriladi).

Oddiy ish jarayoni:

1. **FTP sozlamalari** — xost, port, foydalanuvchi va parolni kiriting; agar kerak bo'lsa FTPS ni yoqing; sinab ko'ring va saqlang.
2. **Papkalar** — bir yoki bir nechta mahalliy papkalarni qo‘shish. Bitta tanlov boradigan yo'lni so'raydi; bir nechta tanlovlar avtomatik ravishda `/FolderName` bilan taqqoslanadi. Mo‘ljallarni keyinroq tahrirlash mumkin.
3. **Jadval** — chastotani tanlang, uni yoqing va saqlang. Rejalashtirish faqat ilova ochiq bo'lganda ishlaydi.
4. **Holat** — qo‘lda ishga tushirish va taraqqiyot, tezlik, jonli jurnal va natijalar xulosasini kuzatish uchun “Hozir sinxronlash” tugmasini bosing.
5. **About** — versiya, ishlab chiquvchi va aloqa maʼlumotlari.

### Xaritaga misol

| Mahalliy | FTP manzili |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` `/backup/projects/src/app.py` sifatida saqlanadi. Serverda bo'sh papkalar yaratilmaydi.

## Ma'lumotlar va jurnallar joylashuvi

- **Manba rejimi**: `data/config.json` va `logs/app.log` loyiha yonida.
- **Windows paketi**: `%APPDATA%\BulutDepolama\data` va `%APPDATA%\BulutDepolama\logs`.
- **Linux paketi**: `$XDG_CONFIG_HOME/bulut-depolama` (odatda `~/.config/bulut-depolama`) `data` va `logs` bilan.
- **Portativ rejim**: `BULUT_PORTABLE=1` muhit o‘zgaruvchisi yoki bajariladigan fayl yonidagi `portable.flag` fayli.
- **Maxsus joylashuv**: `BULUT_DATA_DIR` muhit o‘zgaruvchisi.

## Qurilish

### Windows

Python 3.10+ va PyInstaller talab qilinadi (kerak bo'lsa skript uni o'rnatadi).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Chiqish: `dist\BulutDepolama.exe` (bir faylli GUI). Belgi `assets/icon.ico`dan keladi; `packaging/windows/version_info.txt` versiyasi metama'lumotlari.

### Linux

Ubuntu 22.04+ / Debian-ga asoslangan tizimlarda:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Chiqishlar:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

AppImage-ni ishga tushiring (FUSE kerak):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

FUSE holda:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb` ni o'rnating:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linuxda tizim tepsisi qo'llab-quvvatlanmaydi; oyna harakati va autostart XDG autostart orqali boshqariladi.

## Fayl qachon yuklanadi?

| Vaziyat | Xulq-atvor |
|---|---|
| Serverda fayl yoʻq | Yuklangan (yangi) |
| Hajmi farq qiladi | Yuklangan (yangilangan) |
| Mahalliy modifikatsiya vaqti yangiroq (tolerantlik: 120 s) | Yuklangan (yangilangan) |
| Hajmi va vaqt mosligi | O'tkazib yuborildi |
| Fayl mahalliy sifatida o'chirildi | Serverda saqlanadi |
| Serverdagi qo'shimcha fayl | Saqlangan |

Sana taqqoslash uchun serverda `MFMT` yordami kerak; agar mavjud bo'lmasa, o'lchamlarni taqqoslash ishlatiladi va bu jurnalga yoziladi.

## Tillar

Interfeys 17 tilni qo'llab-quvvatlaydi:

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

Kirish ekrani, sarlavha yoki Xavfsizlik sahifasidan tilni o'zgartiring. Tarjimalar `app/locales/<code>.json` da jonli; etishmayotgan kalitlar ingliz tiliga qaytadi.

## Sinov

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` paketlangan aktivlar va barcha 17 ta mahalliy fayllarni tekshiradi. Loyiha mahalliy FTP serveri (`pyftpdlib`) bilan E2E sinxronlash, bekor qilish va rejalashtiruvchi xatti-harakatlari uchun sinovdan o'tkazildi.

## Loyiha tuzilishi

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

## Xavfsizlik eslatmalari

- Ilova paroli hech qachon diskka yozilmaydi; faqat PBKDF2 tekshirgichi saqlanadi (260 000 iteratsiya, SHA-256).
- FTP paroli AES emas, standart kutubxona HMAC-SHA256 oqim shifrlash (shifrlash-keyin-MAC) bilan shifrlangan. Mahalliy fayllarga kirishdan himoya qiladi; Agar siz **FTPS** ni yoqmasangiz, FTP trafigi shifrlanmagan.
- Beshta muvaffaqiyatsiz kirish urinishi 15 soniyalik kechikishni keltirib chiqaradi.

## Dasturchi

**Tuzuvchi:** Bekir Alisho'g'li  
**Elektron pochta:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Litsenziya

**3 oylik demo litsenziyasi** birinchi ishga tushirishda avtomatik ravishda yaratiladi. Demo paytida barcha modullar ishlaydi. Muddati tugagach, litsenziya ogohlantirishi ko'rsatiladi va to'liq litsenziyani administrator paroli bilan Litsenziyalarni boshqarish panelida ikki sana orasida aniqlash mumkin.

- Litsenziya fayli: `data/license.json`.
- Litsenziya apparat bilan bog'liq emas; faylni boshqa kompyuterga nusxalash mumkin.
- Tizim soatini orqaga siljitish litsenziyani uzaytirmaydi; katta soat o'zgarishlaridan so'ng litsenziya oxirgi ishonchli sana yordamida baholanadi.

![Litsenziya](docs/screenshots/en/07-license.png)