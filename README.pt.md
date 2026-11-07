# Bulut Depolama

Aplicativo moderno de backup FTP unidirecional inspirado no Material Design 3 para Windows e Linux. Construído com Python + Tkinter; sem dependências de tempo de execução externas.

[Türkçe](README.md) • [English](README.en.md) • [Deutsch](README.de.md) • [Français](README.fr.md) • [Azərbaycanca](README.az.md) • [Türkmençe](README.tk.md) • [Oʻzbekcha](README.uz.md) • [Português](README.pt.md) • [Español](README.es.md)  
[Русский](README.ru.md) • [Українська](README.uk.md) • [日本語](README.ja.md) • [简体中文](README.zh.md) • [한국어](README.ko.md) • [Қазақша](README.kk.md) • [Татарча](README.tt.md) • [Тоҷикӣ](README.tg.md)

![Status](docs/screenshots/en/01-dashboard.png)

| Pastas | Cronograma |
|---|---|
| ![Pastas](docs/screenshots/en/02-folders.png) | ![Agendamento](docs/screenshots/en/04-schedule.png) |
| Configurações de FTP | Sobre |
| ![Configurações FTP](docs/screenshots/en/03-ftp.png) | ![Sobre](docs/screenshots/en/06-about.png) |

## Recursos

- **Múltiplas pastas** com um caminho de destino FTP separado para cada pasta.
- **Sincronização unidirecional** do local para o servidor. Nada é excluído do servidor; arquivos extras do servidor são preservados.
- **Upload inteligente**: os arquivos são ignorados quando o tamanho e o horário de modificação correspondem; apenas arquivos novos e alterados são carregados.
- **Agendamento**: de hora em hora (a cada N horas), diariamente, semanalmente (seleção de dia da semana), mensalmente. O próximo tempo de execução é mostrado na UI.
- **Proteção por senha**: o aplicativo pede uma senha na inicialização. As configurações, os detalhes do FTP e a lista de pastas ficam ocultos sem ele. A senha pode ser alterada e o aplicativo pode ser bloqueado manualmente.
- **Armazenamento criptografado**: a senha do FTP é criptografada com uma chave derivada da senha do aplicativo usando PBKDF2 (cifra de fluxo HMAC-SHA256 + MAC) e armazenada em `config.json`.
- **Resiliência**: reconexão e retomada automáticas; uploads parciais são limpos do servidor quando cancelados.
- **FTPS/TLS**, modo passivo/ativo, verificação de certificado, padrões de exclusão e manipulação de arquivos ocultos.
- **17 idiomas**, log ao vivo, bandeja do sistema Windows e inicialização automática do Windows/Linux.
- **Página Sobre** com versão, desenvolvedor e e-mail.

## Início rápido

Python 3.10+ é necessário (testado com 3.13).

```bash
python main.py
```

Na primeira inicialização, você cria uma **senha do aplicativo**. Não pode ser recuperado; exclua `config.json` para redefinir o aplicativo (todas as configurações e a senha de FTP salva são removidas).

Fluxo de trabalho típico:

1. **Configurações de FTP** — insira host, porta, usuário e senha; habilite FTPS se necessário; teste e salve.
2. **Pastas** — adicione uma ou mais pastas locais. Uma única seleção solicita um caminho de destino; múltiplas seleções são mapeadas para `/FolderName` automaticamente. Os destinos podem ser editados posteriormente.
3. **Agendar** — escolha a frequência, habilite e salve. O agendamento é executado apenas enquanto o aplicativo está aberto.
4. **Status** — clique em “Sincronizar agora” para executar manualmente e acompanhar o progresso, a velocidade, o registro ao vivo e o resumo dos resultados.
5. **Sobre** — versão, desenvolvedor e detalhes de contato.

### Exemplo de mapeamento

| Locais | Destino FTP |
|---|---|
| `C:\Users\Ali\Documents` | `/backup/documents` |
| `C:\Projects` | `/backup/projects` |

`C:\Projects\src\app.py` é armazenado como `/backup/projects/src/app.py`. Pastas vazias não são criadas no servidor.

## Locais de dados e registros

- **Modo fonte**: `data/config.json` e `logs/app.log` junto ao projeto.
- **Pacote Windows**: `%APPDATA%\BulutDepolama\data` e `%APPDATA%\BulutDepolama\logs`.
- **Pacote Linux**: `$XDG_CONFIG_HOME/bulut-depolama` (geralmente `~/.config/bulut-depolama`) com `data` e `logs`.
- **Modo portátil**: variável de ambiente `BULUT_PORTABLE=1` ou um arquivo `portable.flag` próximo ao executável.
- **Local personalizado**: variável de ambiente `BULUT_DATA_DIR`.

## Construir

### Windows

Requer Python 3.10+ e PyInstaller (o script instala, se necessário).

```powershell
powershell -ExecutionPolicy Bypass -File packaging\windows\build.ps1
```

Saída: `dist\BulutDepolama.exe` (GUI de arquivo único). O ícone vem de `assets/icon.ico`; metadados de versão de `packaging/windows/version_info.txt`.

### Linux

Em sistemas baseados em Ubuntu 22.04+/Debian:

```bash
sudo apt install python3 python3-venv python3-pip dpkg-dev squashfs-tools \
    libfuse2 desktop-file-utils curl
bash packaging/linux/build.sh
```

Saídas:

-`dist/BulutDepolama-x86_64.AppImage`
-`dist/bulut-depolama_1.0.0_amd64.deb`

Execute o AppImage (é necessário FUSE):

```bash
./dist/BulutDepolama-x86_64.AppImage
```

Sem FUSÍVEL:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./dist/BulutDepolama-x86_64.AppImage
```

Instale o `.deb`:

```bash
sudo apt install ./dist/bulut-depolama_1.0.0_amd64.deb
bulut-depolama
```

O Linux não tem suporte à bandeja do sistema; o comportamento da janela e a inicialização automática são gerenciados por meio da inicialização automática do XDG.

## Quando um arquivo é carregado?

| Situação | Comportamento |
|---|---|
| Arquivo está faltando no servidor | Carregado (novo) |
| O tamanho difere | Carregado (atualizado) |
| O tempo de modificação local é mais recente (tolerância: 120 s) | Carregado (atualizado) |
| Correspondência de tamanho e hora | Ignorado |
| O arquivo foi excluído localmente | Mantido no servidor |
| Arquivo extra no servidor | Preservado |

A comparação de datas requer suporte `MFMT` no servidor; se não estiver disponível, a comparação de tamanho é usada e isso é gravado no log.

## Idiomas

A interface suporta 17 idiomas:

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

Altere o idioma da tela de login, do cabeçalho ou da página Segurança. Traduções ao vivo em `app/locales/<code>.json`; as chaves perdidas voltam para o inglês.

## Teste

```bash
python -m app.crypto
python main.py --version
python main.py --smoke-test
```

`--smoke-test` verifica os ativos empacotados e todos os 17 arquivos de localidade. O projeto foi testado com um servidor FTP local (`pyftpdlib`) para sincronização E2E, cancelamento e comportamento do agendador.

## Estrutura do projeto

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

## Notas de segurança

- A senha do app nunca é gravada no disco; apenas o verificador PBKDF2 é armazenado (260.000 iterações, SHA-256).
- A senha do FTP é criptografada com uma cifra de fluxo HMAC-SHA256 da biblioteca padrão (criptografar e então MAC), não AES. Ele protege contra acesso a arquivos locais; O tráfego FTP em si não é criptografado, a menos que você ative o **FTPS**.
- Cinco tentativas de login malsucedidas acionam um atraso de 15 segundos.

## Desenvolvedor

**Desenvolvedor:** Bekir Alişoğlu  
**E-mail:** [alisoglu@yahoo.com](mailto:alisoglu@yahoo.com)

## Licença

Uma **licença de demonstração de 3 meses** é criada automaticamente na primeira execução. Todos os módulos funcionam durante a demonstração. Quando expirar, um aviso de licença será mostrado e uma licença completa poderá ser definida entre duas datas no painel Gerenciamento de Licenças com a senha do administrador.

- Arquivo de licença: `data/license.json`.
- A licença não está vinculada ao hardware; o arquivo pode ser copiado para outro computador.
- Atrasar o relógio do sistema não estende a licença; após grandes mudanças de relógio, a licença é avaliada usando a última data confiável.

![Licença](docs/screenshots/en/07-license.png)