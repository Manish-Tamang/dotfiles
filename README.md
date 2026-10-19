# dotfiles

Personal config snapshot for quick migration to a new machine. Everything lives
under [`files/`](files/). This is a **reference + restore** set — no secrets are
committed (no tokens, cookies, browser profiles, or SSH keys).

Environment: Windows 11 + WSL2 (Ubuntu), zsh.

## What's here

```
files/
├── shell/            zsh config
│   ├── .zshrc
│   └── .zprofile
├── git/
│   └── .gitconfig
├── editors/          VS Code + Cursor extension lists (JSON)
│   ├── vscode-extensions.json
│   └── cursor-extensions.json
├── browser/
│   └── helium/
│       └── extensions.json   id + name + Chrome Web Store URL
├── fonts/
│   └── jetbrains-mono/        JetBrains Mono .ttf files
└── seelen-ui/        Seelen UI (Windows) settings + themes
    ├── settings.json
    ├── settings_shortcuts.json
    ├── settings_by_app.yml
    ├── start-layout.json
    └── themes/*.slu
```

## Restore / migrate

### Shell + git
```bash
cp files/shell/.zshrc files/shell/.zprofile ~/
cp files/git/.gitconfig ~/
```
> `.gitconfig` uses the GitHub CLI (`gh auth git-credential`) for auth — run
> `gh auth login` on the new machine; no tokens are stored in the repo.

### VS Code / Cursor extensions
```bash
# VS Code
cat files/editors/vscode-extensions.json | python3 -c 'import json,sys;print("\n".join(json.load(sys.stdin)))' | xargs -L1 code --install-extension
# Cursor
cat files/editors/cursor-extensions.json | python3 -c 'import json,sys;print("\n".join(json.load(sys.stdin)))' | xargs -L1 cursor --install-extension
```

### Helium browser extensions
`files/browser/helium/extensions.json` lists each extension with its Chrome Web
Store URL — open each `webstore` link and install. (Only the list is tracked;
the browser profile is never committed.)

### Fonts
Install the `.ttf` files in `files/fonts/jetbrains-mono/`:
- **Windows:** select all → right-click → *Install for all users*.
- **Linux:** `mkdir -p ~/.local/share/fonts && cp files/fonts/jetbrains-mono/*.ttf ~/.local/share/fonts/ && fc-cache -f`

### Seelen UI (Windows)
Copy the settings back into `%AppData%\com.seelen.seelen-ui\` and the `.slu`
theme files into its `themes\` folder, then restart Seelen UI.
