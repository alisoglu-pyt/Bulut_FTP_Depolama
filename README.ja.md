# Bulut Depolama

マテリアル デザイン 3 からインスピレーションを得た最新の Windows および Linux 用 **一方向 FTP バックアップ** アプリケーション。 Python + Tkinter で構築されています。外部ランタイム依存関係はありません。

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![ステータス](docs/screenshots/en/01-dashboard.png)

|フォルダー |スケジュール |
|---|---|
| ![フォルダー](docs/screenshots/en/02-folders.png) | ![スケジュール](docs/screenshots/en/04-schedule.png) |
| FTP設定 |このアプリについて |
| ![FTP 設定](docs/screenshots/en/03-ftp.png) | ![このアプリについて](docs/screenshots/en/06-about.png) |

## 特徴

- **複数のフォルダー**、フォルダーごとに個別の FTP 宛先パスが設定されます。
- **ローカルからサーバーへの一方向の同期**。サーバー上では何も削除されません。余分なサーバー ファイルは保存されます。
- **スマート アップロード**: サイズと変更時間が一致する場合、ファイルはスキップされます。新しいファイルと変更されたファイルのみがアップロードされます。
- **スケジュール**: 時間ごと (N 時間ごと)、毎日、毎週 (平日の選択)、毎月。次回の実行時間が UI に表示されます。
- **パスワード保護**: アプリは起動時にパスワードを要求します。これを行わないと、設定、FTP の詳細、フォルダーの一覧が非表示になります。パスワードを変更したり、アプリを手動でロックしたりできます。
- **暗号化ストレージ**: FTP パスワードは、PBKDF2 (HMAC-SHA256 ストリーム暗号 + MAC) を使用してアプリ パスワードから派生したキーで暗号化され、`config.json` に保存されます。
- **復元力**: 自動的に再接続して再開します。部分的なアップロードは、キャンセルされるとサーバーから消去されます。
- **FTPS/TLS**、パッシブ/アクティブ モード、証明書検証、除外パターン、隠しファイルの処理。
- **17 言語**、ライブ ログ、Windows システム トレイ、Windows/Linux 自動起動。
- **バージョン情報ページ**、バージョン、開発者、電子メールが記載されています。

## クイックスタート

Python 3.10 以降が必要です (3.13 でテスト済み)。

```bash
python main.py
```

最初の起動時に **アプリケーション パスワード**を作成します。それは回復できません。 `config.json` を削除してアプリをリセットします (すべての設定と保​​存された FTP パスワードが削除されます)。

一般的なワークフロー:

1. **FTP 設定** — ホスト、ポート、ユーザー、パスワードを入力します。必要に応じて FTPS を有効にします。テストして保存します。
2. **フォルダー** — 1 つ以上のローカル フォルダーを追加します。単一の選択では、宛先パスを要求されます。複数の選択は `/FolderName` に自動的にマップされます。宛先は後で編集できます。
3. **スケジュール** — 頻度を選択し、有効にして保存します。スケジュールはアプリが開いている間のみ実行されます。
4. **ステータス** — 「今すぐ同期」をクリックして手動で実行し、進行状況、速度、ライブログ、および結果の概要を追跡します。
5. **このアプリについて** — バージョン、開発者、連絡先の詳細。

### マッピングの例

|ローカル | FTP 宛先 |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` は `/backup/projects/src/app.py` として保存されます。空のフォルダーはサーバー上に作成されません。

## データとログの場所

- **ソース モード**: プロジェクトの隣にある `data/config.json` および `logs/app.log`。
- **Windows パッケージ**: `%APPDATA%\BulutDepolama\data` および `%APPDATA%\BulutDepolama\logs`。
- **Linux パッケージ**: `$XDG_CONFIG_HOME/bulut-depolama` (通常は `~/.config/bulut-depolama`) と `data` および `logs`。
- **ポータブル モード**: `BULUT_PORTABLE=1` 環境変数、または実行可能ファイルの隣にある `portable.flag` ファイル。
- **カスタムの場所**: `BULUT_DATA_DIR` 環境変数。

## ビルドする

### Windows

Python 3.10 以降と PyInstaller が必要です (必要に応じてスクリプトによってインストールされます)。

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

出力: `dist\BulutDepolama.exe` (単一ファイル GUI)。アイコンは `assets/icon.ico` からのものです。 `packaging/windows/version_info.txt` からのバージョン メタデータ。

### Linux

Ubuntu 22.04+ / Debian ベースのシステムの場合:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

出力:

- `dist/BulutDepolama-x86_64.AppImage`
- `dist/bulut-depolama_1.0.0_amd64.deb`

AppImage を実行します (FUSE が必要)。

```bash
./dist/BulutDepolama-x86_64.AppImage
```

ヒューズなし:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

`.deb` をインストールします。

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux にはシステム トレイ サポートがありません。ウィンドウの動作と自動起動は、XDG 自動起動によって管理されます。

## ファイルはいつアップロードされますか?

|状況 |行動 |
|---|---|
|ファイルがサーバー上にありません |アップロード済み (新規) |
|サイズが異なります |アップロード (更新) |
|ローカル変更時間が新しい (許容範囲: 120 秒) |アップロード (更新) |
|サイズと時間が一致します |スキップされました |
|ファイルはローカルで削除されました |サーバー上に保存 |
|サーバー上の余分なファイル |保存済み |

日付比較にはサーバー上の `MFMT` サポートが必要です。利用できない場合は、サイズ比較が使用され、これがログに書き込まれます。

## 言語

インターフェイスは 17 の言語をサポートしています。

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

ログイン画面、ヘッダー、またはセキュリティ ページから言語を変更します。翻訳は `app/locales/<code>.json` にあります。キーが見つからない場合は英語に戻ります。

## テスト

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` は、パッケージ化されたアセットと 17 個のロケール ファイルすべてを検証します。プロジェクトは、ローカル FTP サーバー (`pyftpdlib`) を使用して、E2E 同期、キャンセル、スケジューラの動作についてテストされています。

## プロジェクトの構造

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

## セキュリティに関する注意事項

- アプリのパスワードがディスクに書き込まれることはありません。 PBKDF2 ベリファイアのみが保存されます (260,000 回の反復、SHA-256)。
- FTP パスワードは、AES ではなく、標準ライブラリの HMAC-SHA256 ストリーム暗号 (encrypt-then-MAC) で暗号化されます。ローカル ファイル アクセスから保護します。 **FTPS** を有効にしない限り、FTP トラフィック自体は暗号化されません。
- ログイン試行が 5 回失敗すると、15 秒の遅延が発生します。

## 開発者

**開発者:** Bekir Alişoğlu  
**電子メール:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## ライセンス

**3 か月のデモ ライセンス**は、初回実行時に自動的に作成されます。デモ中はすべてのモジュールが動作します。有効期限が切れると、ライセンスの警告が表示され、管理者パスワードを使用してライセンス管理パネルで 2 つの日付の間で完全なライセンスを定義できます。

- ライセンスファイル: `data/license.json`.
- ライセンスはハードウェアに関連付けられていません。ファイルは別のコンピュータにコピーできます。
- システム クロックを戻してもライセンスは延長されません。時計が大幅に変更された後、ライセンスは最後に信頼された日付を使用して評価されます。

![ライセンス](docs/screenshots/en/07-license.png)