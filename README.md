```
             _                    _
  __ _  ___ | | ___  ___ ___   __| | ___  ___
 / _` |/ _ \| |/ _ \/ __/ _ \ / _` |/ _ \/ __|
| (_| | (_) | |  __/ (_| (_) | (_| |  __/\__ \
 \__, |\___/|_|\___|\___\___/ \__,_|\___||___/
 |___/           d o t f i l e s
```

# My dotfiles

My personal config backup, so when I set up a new machine I don't have to
remember how I had everything wired up. Organized at the repo root by topic.

## My setup

| Thing | What I use |
|-------|-----------|
| OS | Windows 11 + **WSL2 (Ubuntu)** |
| Shell | **zsh** + [oh-my-zsh](https://ohmyz.sh) |
| Prompt | **Pure** (`prompt pure`) |
| Jumping around | **zoxide** (`z`) |
| Fuzzy finding | **fzf** |
| Package manager | **Homebrew** (linuxbrew) |
| Node | **nvm** + **bun** + pnpm/corepack |
| Editors | VS Code + Cursor |
| Browser | Helium + [Minimal YouTube](https://github.com/Manish-Tamang/minimal-youtube) |
| Font | JetBrains Mono |
| Windows desktop | Seelen UI |
| Windows notch | [Nusa](https://rama2555.github.io/nusa/) (native app) |

## What's in here

```
.
├── Brewfile                  # everything Homebrew knows: brew/cask/vscode/winget/npm
├── zsh/
│   ├── .zshrc                # oh-my-zsh + pure + zoxide + fzf + nvm + bun
│   └── .zprofile             # brew shellenv
├── git/
│   ├── .gitconfig
│   └── gitignore_global
├── editors/
│   ├── vscode-settings.json      # VS Code user settings (classic flat UI, no API keys)
│   ├── vscode-extensions.json
│   └── cursor-extensions.json
├── browser/helium/
│   └── extensions.json       # id + name + Chrome Web Store URL (+ unpacked GitHub ones)
├── fonts/jetbrains-mono/     # the .ttf files
├── seelen-ui/                # Seelen UI settings + my .slu themes
└── nusa/
    └── water.txt             # Nusa's water reminder (bottle, goal, reminder hours)
```

> The **Brewfile** is the one file to rule them all — it also captured my Windows
> apps via winget (Figma, OBS, Spotify, MongoDB, Steam, Seelen UI, 1Password…),
> so it doubles as my "apps I'd otherwise forget" list.

## Fresh machine setup

```bash
git clone https://github.com/Manish-Tamang/dotfiles.git ~/dotfiles
cd ~/dotfiles
```

**1. Shell + git**
```bash
cp zsh/.zshrc zsh/.zprofile ~/
mkdir -p ~/.config/git
cp git/.gitconfig ~/
cp git/gitignore_global ~/.config/git/ignore
# oh-my-zsh + the pieces I use
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/zsh-users/zsh-syntax-highlighting ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
```

**2. Everything from Homebrew (incl. pure, fzf, zoxide, bun, nvm)**
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew bundle --file=Brewfile          # on WSL: installs brew formulae/casks
# (the winget lines are for the Windows side — run `brew bundle` from there, or install manually)
```
Then `gh auth login` to sign in to GitHub.

**3. Editor extensions**
```bash
python3 -c 'import json;print("\n".join(json.load(open("editors/vscode-extensions.json"))))' | xargs -L1 code --install-extension
python3 -c 'import json;print("\n".join(json.load(open("editors/cursor-extensions.json"))))' | xargs -L1 cursor --install-extension
```
Then copy `editors/vscode-settings.json` to `%AppData%\Code\User\settings.json` and re-add your WakaTime key
(it's kept out of the repo). It sets `workbench.experimental.modernUI: false`, which turns off VS Code's floating rounded
panels so it looks flat and seamless like Cursor.

**4. Fonts**
```bash
# Linux
mkdir -p ~/.local/share/fonts && cp fonts/jetbrains-mono/*.ttf ~/.local/share/fonts/ && fc-cache -f
# Windows: select the .ttf files → right-click → Install for all users
```

**5. Helium extensions** — open each `webstore` URL in `browser/helium/extensions.json` and install.

[Minimal YouTube](https://github.com/Manish-Tamang/minimal-youtube) isn't on the Web Store, so load it unpacked:
download `minimal-youtube-vX.Y.Z.zip` from the [latest release](https://github.com/Manish-Tamang/minimal-youtube/releases/latest),
unzip it to a folder you'll keep, open `chrome://extensions`, turn on **Developer mode** → **Load unpacked** → pick the folder.

**6. Seelen UI (Windows)** — copy `seelen-ui/*` into `%AppData%\com.seelen.seelen-ui\` and the `.slu` files into its `themes\` folder, then restart Seelen.

**7. Nusa (native Windows app)** — not on winget, so grab [`NusaSetup.zip`](https://github.com/rama2555/nusa/releases/latest/download/NusaSetup.zip)
from the [Nusa site](https://rama2555.github.io/nusa/), unzip and run the installer. Then copy `nusa/water.txt` into
`%AppData%\Nusa\` and restart Nusa. That file is its only config (500 ml bottle, 2000 ml goal, reminders 08:00–17:00);
the file shelf lives in `%LocalAppData%\Nusa\Shelf\` and isn't backed up.

## Refreshing this repo later

```bash
brew bundle dump --file=Brewfile --force     # re-capture packages/apps
code --list-extensions                       # re-grab editor extensions
cp /mnt/c/Users/ACER/AppData/Roaming/Nusa/water.txt nusa/   # re-grab Nusa config (from WSL)
```
