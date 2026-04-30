# proton-pass-rofi

A small `rofi` frontend for the official Proton Pass CLI, `pass-cli`.

It is inspired by `rofi-rbw`, but talks directly to Proton's CLI:

- list vaults and active items
- search items directly in the first `rofi` prompt
- copy password, username, email, URL, note, TOTP, or a custom scalar field
- type username, password, or username + tab + password
- open an item's URL
- generate a random password or passphrase and copy it
- start `pass-cli login` or `pass-cli logout`

## Requirements

- `pass-cli`
- `rofi`
- one clipboard backend: `wl-copy`, `xclip`, or `xsel`
- optional typing backend: `wtype` or `xdotool`
- optional notifications: `notify-send`
- optional terminal for login/logout: `rofi-sensible-terminal` or another terminal

Install Proton Pass CLI from the official docs:

```sh
curl -fsSL https://proton.me/download/pass-cli/install.sh | bash
```

Log in before using the launcher:

```sh
pass-cli login
```

## Usage

Run directly from this repository:

```sh
./proton-pass-rofi
```

The first prompt is the searchable item list. Pressing `Enter` immediately
types the selected password and exits.

Default item-list keybindings match `rofi-rbw`:

- `Enter` / `Alt+3`: type password
- `Alt+1`: type username, tab, password, and copy TOTP if available
- `Alt+2`: type username
- `Alt+u`: copy username
- `Alt+p` / `Alt+c`: copy password
- `Alt+t`: copy TOTP
- `Alt+m`: open the field/action menu
- `Alt+s`: refresh the cached item list

Or install it somewhere on your `PATH`:

```sh
install -Dm755 proton-pass-rofi ~/.local/bin/proton-pass-rofi
```

Then bind `proton-pass-rofi` in your window manager.

## Configuration

Environment variables:

- `PROTON_PASS_ROFI_PASS_CLI`: override the `pass-cli` executable
- `PROTON_PASS_ROFI_ROFI`: override the `rofi` executable
- `PROTON_PASS_ROFI_TERMINAL`: override the terminal used for login/logout
- `PROTON_PASS_ROFI_CLEAR_SECONDS`: clipboard clear timeout, default `0`
- `PROTON_PASS_ROFI_CACHE`: set to `0` to disable cached item metadata

Equivalent flags:

```sh
proton-pass-rofi --pass-cli /path/to/pass-cli --rofi /path/to/rofi --terminal foot --clear-seconds 30
```

Set `--clear-seconds 0` to disable clipboard clearing.

Item metadata is cached in `$XDG_CACHE_HOME/proton-pass-rofi/items.json` so
launcher startup stays fast. The cache contains display/lookup metadata only,
not item passwords. Use `Alt+s`, `--refresh-cache`, or delete the cache file
after changing vault contents.

To debug Proton Pass CLI parsing without opening rofi:

```sh
proton-pass-rofi --dump-items
```