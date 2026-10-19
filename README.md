```
 ██████╗  ██████╗ ██╗     ███████╗
██╔════╝ ██╔═══██╗██║     ██╔════╝
██║  ███╗██║   ██║██║     █████╗
██║   ██║██║   ██║██║     ██╔══╝
╚██████╔╝╚██████╔╝███████╗███████╗
 ╚═════╝  ╚═════╝ ╚══════╝╚══════╝
        d o t f i l e s
```

# My dotfiles

This is my personal config backup so that when I nuke a machine or move to a new
one, I don't have to remember what I had. Everything lives at the repo root by
topic. **No secrets are committed** — no tokens, cookies, browser profiles,
SSH/WakaTime keys, or `gh` auth.

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
| Browser | Helium |
| Font | JetBrains Mono |
| Windows desktop | Seelen UI |

## What's in here

```
.
├── Brewfile                  # everything Homebrew knows: brew/cask/vscode/winget/npm
├── zsh/
│   ├── .zshrc                # oh-my-zsh + pure + zoxide + fzf + nvm + bun
│   └── .zprofile             # brew shellenv
├── git/
│   ├── .gitconfig            # auth via gh CLI, no tokens
│   └── gitignore_global      # my global ignore (~/.config/git/ignore)
├── editors/
│   ├── vscode-extensions.json
│   └── cursor-extensions.json
├── browser/helium/
│   └── extensions.json       # id + name + Chrome Web Store URL (list only)
├── fonts/jetbrains-mono/     # the .ttf files
└── seelen-ui/                # Seelen UI settings + my .slu themes
```

> The **Brewfile** is the one file to rule them all — it also captured my Windows
> apps via winget (Figma, OBS, Spotify, MongoDB, Steam, Seelen UI, 1Password…),
> so it's my "apps I'd otherwise forget" list too.

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
# (the winget lines in the Brewfile are for the Windows side — run `brew bundle` from there, or install manually)
```
> `gh auth login` after — my `.gitconfig` uses the gh credential helper, nothing stored.

**3. Editor extensions**
```bash
python3 -c 'import json;print("\n".join(json.load(open("editors/vscode-extensions.json"))))' | xargs -L1 code --install-extension
python3 -c 'import json;print("\n".join(json.load(open("editors/cursor-extensions.json"))))' | xargs -L1 cursor --install-extension
```

**4. Fonts**
```bash
# Linux
mkdir -p ~/.local/share/fonts && cp fonts/jetbrains-mono/*.ttf ~/.local/share/fonts/ && fc-cache -f
# Windows: select the .ttf files → right-click → Install for all users
```

**5. Helium extensions** — open each `webstore` URL in `browser/helium/extensions.json` and install.

**6. Seelen UI (Windows)** — copy `seelen-ui/*` into `%AppData%\com.seelen.seelen-ui\` and the `.slu` files into its `themes\` folder, then restart Seelen.

## Refreshing this repo later

```bash
brew bundle dump --file=Brewfile --force     # re-capture packages/apps
code --list-extensions   > /tmp/vsc   # (then reformat to JSON)
```

## Not committed on purpose

`~/.wakatime.cfg` (API key), `~/.config/gh` (auth token), browser profile data,
and anything matching `.gitignore`. These stay on my machine only.
