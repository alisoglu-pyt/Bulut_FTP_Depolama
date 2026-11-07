# Bulut Depolama

현대적인 Material Design 3에서 영감을 받은 Windows 및 Linux용 **단방향 FTP 백업** 애플리케이션입니다. Python + Tkinter로 제작되었습니다. 외부 런타임 종속성이 없습니다.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![상태](docs/screenshots/en/01-dashboard.png)

| 폴더 | 일정 |
|---|---|
| ![폴더](docs/screenshots/en/02-folders.png) | ![일정](docs/screenshots/en/04-schedule.png) |
| FTP 설정 | 소개 |
| ![FTP 설정](docs/screenshots/en/03-ftp.png) | ![정보](docs/screenshots/en/06-about.png) |

## 기능

- 각 폴더에 대해 별도의 FTP 대상 경로가 있는 **여러 폴더**.
- 로컬에서 서버로 **단방향 동기화**. 서버에서는 아무것도 삭제되지 않습니다. 추가 서버 파일은 보존됩니다.
- **스마트 업로드**: 크기와 수정 시간이 일치하면 파일을 건너뜁니다. 새로운 파일과 변경된 파일만 업로드됩니다.
- **스케줄링**: 시간별(N시간마다), 매일, 매주(요일 선택), 매월. 다음 실행 시간이 UI에 표시됩니다.
- **비밀번호 보호**: 앱 시작 시 비밀번호를 묻습니다. 설정, FTP 세부 정보 및 폴더 목록은 해당 항목 없이 숨겨집니다. 비밀번호를 변경하고 앱을 수동으로 잠글 수 있습니다.
- **암호화된 저장소**: FTP 비밀번호는 PBKDF2(HMAC-SHA256 스트림 암호 + MAC)를 사용하여 앱 비밀번호에서 파생된 키로 암호화되어 `config.json`에 저장됩니다.
- **복원력**: 자동 재연결 및 재개; 부분 업로드는 취소되면 서버에서 정리됩니다.
- **FTPS/TLS**, 패시브/액티브 모드, 인증서 확인, 제외 패턴 및 숨김 파일 처리.
- **17개 언어**, 라이브 로그, Windows 시스템 트레이 및 Windows/Linux 자동 시작.
- 버전, 개발자, 이메일이 포함된 **정보 페이지**.

## 빠른 시작

Python 3.10+가 필요합니다(3.13에서 테스트됨).

```bash
python main.py
```

처음 실행 시 **애플리케이션 비밀번호**를 생성합니다. 복구할 수 없습니다. 앱을 재설정하려면 `config.json`를 삭제하세요(모든 설정과 저장된 FTP 비밀번호가 제거됩니다).

일반적인 작업 흐름:

1. **FTP 설정** — 호스트, 포트, 사용자 및 비밀번호를 입력합니다. 필요한 경우 FTPS를 활성화합니다. 테스트하고 저장하세요.
2. **폴더** — 하나 이상의 로컬 폴더를 추가합니다. 단일 선택으로 대상 경로를 묻습니다. 여러 선택 항목이 자동으로 `/FolderName`에 매핑됩니다. 목적지는 나중에 편집할 수 있습니다.
3. **일정** — 빈도를 선택하고 활성화한 후 저장합니다. 일정은 앱이 열려 있는 동안에만 실행됩니다.
4. **상태** — "지금 동기화"를 클릭하여 수동으로 실행하고 진행 상황, 속도, 실시간 로그 및 결과 요약을 확인하세요.
5. **정보** — 버전, 개발자 및 연락처 정보.

### 매핑 예

| 지역 | FTP 대상 |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py`는 `/backup/projects/src/app.py`로 저장됩니다. 서버에 빈 폴더가 생성되지 않습니다.

## 데이터 및 로그 위치

- **소스 모드**: 프로젝트 옆에 있는 `data/config.json` 및 `logs/app.log`.
- **Windows 패키지**: `%APPDATA%\BulutDepolama\data` 및 `%APPDATA%\BulutDepolama\logs`.
- **Linux 패키지**: `$XDG_CONFIG_HOME/bulut-depolama`(일반적으로 `~/.config/bulut-depolama`)(`data` 및 `logs` 포함).
- **휴대용 모드**: `BULUT_PORTABLE=1` 환경 변수 또는 실행 파일 옆에 있는 `portable.flag` 파일.
- **사용자 정의 위치**: `BULUT_DATA_DIR` 환경 변수.

## 빌드

### Windows

Python 3.10+ 및 PyInstaller가 필요합니다(필요한 경우 스크립트가 이를 설치합니다).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

출력: `dist\BulutDepolama.exe`(단일 파일 GUI). 아이콘은 `assets/icon.ico`에서 유래되었습니다. `packaging/windows/version_info.txt`의 버전 메타데이터입니다.

### Linux

Ubuntu 22.04+/Debian 기반 시스템:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

출력:

-`dist/BulutDepolama-x86_64.AppImage`
-`dist/bulut-depolama_1.0.0_amd64.deb`

AppImage를 실행합니다(FUSE 필요).

```bash
./dist/BulutDepolama-x86_64.AppImage
```

퓨즈가 없는 경우:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb`를 설치합니다:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux에는 시스템 트레이가 지원되지 않습니다. 창 동작 및 자동 시작은 XDG 자동 시작을 통해 관리됩니다.

## 파일은 언제 업로드되나요?

| 상황 | 행동 |
|---|---|
| 서버에 파일이 없습니다 | 업로드됨(신규) |
| 크기가 다릅니다 | 업로드됨(업데이트됨) |
| 로컬 수정 시간이 더 최신임(공차: 120초) | 업로드됨(업데이트됨) |
| 크기 및 시간 일치 | 건너뛰었습니다 |
| 파일이 로컬에서 삭제되었습니다 | 서버에 보관됨 |
| 서버의 추가 파일 | 보존 |

날짜 비교에는 서버에서 `MFMT` 지원이 필요합니다. 사용할 수 없는 경우 크기 비교가 사용되며 이는 로그에 기록됩니다.

## 언어

인터페이스는 17개 언어를 지원합니다.

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

로그인 화면, 헤더 또는 보안 페이지에서 언어를 변경하세요. 번역은 `app/locales/<code>.json`에 있습니다. 누락된 키는 영어로 대체됩니다.

## 테스트

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test`는 패키지된 자산과 17개의 로케일 파일을 모두 확인합니다. 이 프로젝트는 E2E 동기화, 취소 및 스케줄러 동작에 대해 로컬 FTP 서버(`pyftpdlib`)를 사용하여 테스트되었습니다.

## 프로젝트 구조

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

## 보안 참고 사항

- 앱 비밀번호는 디스크에 절대 기록되지 않습니다. PBKDF2 검증자만 저장됩니다(260,000회 반복, SHA-256).
- FTP 비밀번호는 AES가 아닌 표준 라이브러리 HMAC-SHA256 스트림 암호(encrypt-then-MAC)로 암호화됩니다. 로컬 파일 액세스로부터 보호합니다. **FTPS**를 활성화하지 않으면 FTP 트래픽 자체는 암호화되지 않습니다.
- 5번의 로그인 시도 실패 시 15초 지연이 발생합니다.

## 개발자

**개발자:** 베키르 알리쇼글루(Bekir Alişoğlu)  
**이메일:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## 라이센스

**3개월 데모 라이센스**는 처음 실행 시 자동으로 생성됩니다. 모든 모듈은 데모 중에 작동합니다. 만료되면 라이센스 경고가 표시되며 관리자 비밀번호를 사용하여 라이센스 관리 패널에서 두 날짜 사이에 전체 라이센스를 정의할 수 있습니다.

- 라이센스 파일: `data/license.json`.
- 라이센스는 하드웨어에 묶여 있지 않습니다. 파일을 다른 컴퓨터에 복사할 수 있습니다.
- 시스템 시계를 뒤로 이동해도 라이센스가 연장되지는 않습니다. 시계가 크게 변경된 후에는 마지막으로 신뢰할 수 있는 날짜를 사용하여 라이센스가 평가됩니다.

![라이센스](docs/screenshots/en/07-license.png)