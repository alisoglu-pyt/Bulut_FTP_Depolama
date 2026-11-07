# Bulut Depolama

Application de **sauvegarde FTP unidirectionnelle** moderne inspirée de Material Design 3 pour Windows et Linux. Construit avec Python + Tkinter ; aucune dépendance d'exécution externe.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![État](docs/screenshots/en/01-dashboard.png)

| Dossiers | Calendrier |
|---|---|
| ![Dossiers](docs/screenshots/en/02-folders.png) | ![Planification](docs/screenshots/en/04-schedule.png) |
| Paramètres FTP | À propos |
| ![Paramètres FTP](docs/screenshots/en/03-ftp.png) | ![À propos](docs/screenshots/en/06-about.png) |

## Fonctionnalités

- **Plusieurs dossiers** avec un chemin de destination FTP distinct pour chaque dossier.
- **Synchronisation unidirectionnelle** du local au serveur. Rien n'est jamais supprimé sur le serveur ; les fichiers de serveur supplémentaires sont conservés.
- **Téléchargement intelligent** : les fichiers sont ignorés lorsque la taille et l'heure de modification correspondent ; seuls les fichiers nouveaux et modifiés sont téléchargés.
- **Planification** : horaire (toutes les N heures), quotidienne, hebdomadaire (sélection du jour de la semaine), mensuelle. La prochaine exécution est affichée dans l'interface utilisateur.
- **Protection par mot de passe** : l'application demande un mot de passe au démarrage. Les paramètres, les détails FTP et la liste des dossiers sont masqués sans cela. Le mot de passe peut être modifié et l'application peut être verrouillée manuellement.
- **Stockage crypté** : le mot de passe FTP est crypté avec une clé dérivée du mot de passe de l'application à l'aide de PBKDF2 (chiffrement de flux HMAC-SHA256 + MAC) et stocké dans `config.json`.
- **Résilience** : reconnexion et reprise automatiques ; les téléchargements partiels sont nettoyés du serveur lorsqu'ils sont annulés.
- **FTPS/TLS**, mode passif/actif, vérification des certificats, modèles d'exclusion et gestion des fichiers cachés.
- **17 langues**, journal en direct, barre d'état système Windows et démarrage automatique Windows/Linux.
- **Page À propos** avec version, développeur et e-mail.

## Démarrage rapide

Python 3.10+ est requis (testé avec 3.13).

```bash
python main.py
```

Au premier lancement, vous créez un **mot de passe d'application**. Il ne peut pas être récupéré ; supprimez `config.json` pour réinitialiser l'application (tous les paramètres et le mot de passe FTP enregistré sont supprimés).

Flux de travail typique :

1. **Paramètres FTP** — saisissez l'hôte, le port, l'utilisateur et le mot de passe ; activez FTPS si nécessaire ; tester et sauvegarder.
2. **Dossiers** : ajoutez un ou plusieurs dossiers locaux. Une seule sélection demande un chemin de destination ; plusieurs sélections sont automatiquement mappées sur `/FolderName`. Les destinations peuvent être modifiées ultérieurement.
3. **Programme** — choisissez la fréquence, activez-la et enregistrez. La planification s'exécute uniquement lorsque l'application est ouverte.
4. **Statut** — cliquez sur « Synchroniser maintenant » pour exécuter manuellement et suivre la progression, la vitesse, le journal en direct et le résumé des résultats.
5. **À propos** — version, développeur et coordonnées.

### Exemple de mappage

| Locale | Destination FTP |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` est stocké sous le nom `/backup/projects/src/app.py`. Les dossiers vides ne sont pas créés sur le serveur.

## Emplacements des données et des journaux

- **Mode source** : `data/config.json` et `logs/app.log` à côté du projet.
- **Package Windows** : `%APPDATA%\BulutDepolama\data` et `%APPDATA%\BulutDepolama\logs`.
- **Package Linux** : `$XDG_CONFIG_HOME/bulut-depolama` (généralement `~/.config/bulut-depolama`) avec `data` et `logs`.
- **Mode portable** : variable d'environnement `BULUT_PORTABLE=1` ou un fichier `portable.flag` à côté de l'exécutable.
- **Emplacement personnalisé** : variable d'environnement `BULUT_DATA_DIR`.

## Construire

### Windows

Nécessite Python 3.10+ et PyInstaller (le script l'installe si nécessaire).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Sortie : `dist\BulutDepolama.exe` (interface graphique à fichier unique). L'icône provient de `assets/icon.ico` ; métadonnées de version de `packaging/windows/version_info.txt`.

### Linux

Sur les systèmes Ubuntu 22.04+ / Debian :

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Sorties :

-`dist/BulutDepolama-x86_64.AppImage`
-`dist/bulut-depolama_1.0.0_amd64.deb`

Exécutez AppImage (FUSE requis) :

```bash
./dist/BulutDepolama-x86_64.AppImage
```

Sans FUSE :

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

Installez le `.deb` :

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

Linux ne prend pas en charge la barre d'état système ; le comportement de la fenêtre et le démarrage automatique sont gérés via le démarrage automatique XDG.

## Quand un fichier est-il téléchargé ?

| Situation | Comportement |
|---|---|
| Le fichier est manquant sur le serveur | Téléchargé (nouveau) |
| La taille diffère | Téléchargé (mis à jour) |
| L'heure de modification locale est plus récente (tolérance : 120 s) | Téléchargé (mis à jour) |
| Correspondance de taille et d'heure | Sauté |
| Le fichier a été supprimé localement | Conservé sur le serveur |
| Fichier supplémentaire sur le serveur | Conservé |

La comparaison des dates nécessite la prise en charge de `MFMT` sur le serveur ; s'il n'est pas disponible, la comparaison de taille est utilisée et ceci est écrit dans le journal.

## Langues

L'interface prend en charge 17 langues :

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

Changez la langue depuis l'écran de connexion, l'en-tête ou la page Sécurité. Les traductions sont disponibles dans `app/locales/<code>.json` ; les clés manquantes reviennent à l'anglais.

## Tester

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` vérifie les ressources packagées et les 17 fichiers de paramètres régionaux. Le projet a été testé avec un serveur FTP local (`pyftpdlib`) pour le comportement de synchronisation, d'annulation et de planification E2E.

## Structure du projet

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

## Notes de sécurité

- Le mot de passe de l'application n'est jamais écrit sur le disque ; seul le vérificateur PBKDF2 est stocké (260 000 itérations, SHA-256).
- Le mot de passe FTP est chiffré avec un chiffrement de flux HMAC-SHA256 de bibliothèque standard (crypter-puis-MAC), et non AES. Il protège contre l'accès aux fichiers locaux ; Le trafic FTP lui-même n'est pas chiffré, sauf si vous activez **FTPS**.
- Cinq tentatives de connexion infructueuses déclenchent un délai de 15 secondes.

## Développeur

**Développeur:** Bekir Alişoğlu  
**E-mail :** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Licence

Une **licence de démonstration de 3 mois** est créée automatiquement lors de la première exécution. Tous les modules fonctionnent pendant la démo. À son expiration, un avertissement de licence s'affiche et une licence complète peut être définie entre deux dates dans le panneau Gestion des licences avec le mot de passe administrateur.

- Fichier de licence: `data/license.json`.
- La licence n'est pas liée au matériel ; le fichier peut être copié sur un autre ordinateur.
- Le fait de reculer l’horloge du système ne prolonge pas la licence ; après de grands changements d'horloge, la licence est évaluée en utilisant la dernière date fiable.

![Licence](docs/screenshots/en/07-license.png)