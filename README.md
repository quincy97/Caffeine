# ☕ Caffeine

A terminal-based keep-awake utility for macOS — replicating [Caffeine.app](https://www.caffeine-app.net) entirely from the command line.

No menu bar icon, no GUI, no dependencies. Just a single zsh script that prevents your Mac from sleeping. And keep it simple too!

---

## Features

- **Interactive TUI** — arrow-key menu to pick duration, live countdown timer
- **CLI mode** — scriptable `on` / `off` / `toggle` / `status` commands
- **Timed durations** — 5m, 10m, 15m, 30m, 1h, 2h, 5h, or indefinite
- **Live status screen** — real-time countdown with in-session controls
- **Timer expiry handling** — notifies when timer ends, option to restart or pick a new duration
- **Quit confirmation** — warns you before quitting while Caffeine is active

---

## Installation

### Quick install

```bash
curl -fsSL https://raw.githubusercontent.com/quincy97/Caffeine/main/caffeine -o /usr/local/bin/caffeine
chmod +x /usr/local/bin/caffeine
```

### Manual install

```bash
git clone https://github.com/quincy97/Caffeine.git
cd Caffeine
cp caffeine /usr/local/bin/caffeine
chmod +x /usr/local/bin/caffeine
```

> **Note:** You can place the script anywhere on your `$PATH`. `/usr/local/bin` is just a common choice.

---

## Usage

### Interactive mode

```bash
caffeine
```

Opens a TUI menu where you can select a duration with arrow keys:

```
  ┌─────────────────────────────────────┐
  │           ☕  C A F F E I N E        │
  │       Keep your Mac awake            │
  └─────────────────────────────────────┘

  Status:  😴 OFF

  ─────────────────────────────────────

    ❯ Turn ON  → Indefinite
      Turn ON  → 5 minutes
      Turn ON  → 10 minutes
      Turn ON  → 15 minutes
      Turn ON  → 30 minutes
      Turn ON  → 1 hour
      Turn ON  → 2 hours
      Turn ON  → 5 hours

  ↑↓ Navigate  ⏎ Select  q Quit
```

After selecting, a live status screen shows the countdown:

```
  ☕ ON — 29m 45s remaining

  ─────────────────────────────────────

  ⚠  Keep this terminal open.
  Closing the terminal will stop Caffeine.

  o Turn off    m Change duration    q Quit
```

### CLI mode

```bash
caffeine on           # Stay awake indefinitely
caffeine on 30m       # Stay awake for 30 minutes
caffeine on 2h        # Stay awake for 2 hours
caffeine off          # Allow sleep again
caffeine toggle       # Toggle on/off
caffeine status       # Check current state
caffeine help         # Show help
```

### Available durations

| Flag | Duration |
|------|----------|
| `5m` | 5 minutes |
| `10m` | 10 minutes |
| `15m` | 15 minutes |
| `30m` | 30 minutes |
| `1h` | 1 hour |
| `2h` | 2 hours |
| `5h` | 5 hours |
| *(none)* | Indefinite |

---

## Keyboard shortcuts

### Interactive menu

| Key | Action |
|-----|--------|
| `↑` `↓` | Navigate options |
| `⏎` | Select |
| `q` | Quit |

### Live status screen

| Key | Action |
|-----|--------|
| `o` | Turn off |
| `m` | Change duration (back to menu) |
| `q` | Quit (with confirmation) |

### Timer expired screen

| Key | Action |
|-----|--------|
| `r` | Restart with same duration |
| `m` | Pick a new duration |
| `q` | Quit |

---

## How it works

Caffeine wraps macOS's built-in `caffeinate` command with flags `-dims`:

| Flag | Prevents |
|------|----------|
| `-d` | Display sleep |
| `-i` | Idle system sleep |
| `-m` | Disk sleep |
| `-s` | System sleep (on AC power) |

State is tracked via temporary files in `/tmp/`:

| File | Purpose |
|------|---------|
| `caffeine.pid` | PID of the `caffeinate` process |
| `caffeine.end` | Unix timestamp when timer expires |
| `caffeine.dur` | Last-used duration label (for restart) |

---

## Requirements

- macOS (any version with `caffeinate` — 10.8 Mountain Lion or later)
- zsh (default shell on macOS since Catalina)

---

## Uninstall

```bash
rm /usr/local/bin/caffeine
rm -f /tmp/caffeine.pid /tmp/caffeine.end /tmp/caffeine.dur
```

---

## Licence

MIT
