# ❖ Glass

**A liquid-glass desktop for the web — one HTML file, no build step, no dependencies.**

🔗 **Live demo:** https://solid-apps.github.io/glass/

Glass is a self-contained desktop environment in a single `index.html` (~110 KB): a real
window manager (drag, minimize-to-dock, zoom, Overview), menu bar, dock with magnification,
⌘Space search, notifications, Control Center, and a suite of built-in apps — Files, Compass
(browser), Notes, Terminal, Music (real-time WebAudio synth), Photos (procedural canvas art),
Weather, Clock, Calculator, Settings, and Trash — all backed by an in-memory virtual
filesystem.

Open the file. That's the install.

## Things to try

- Double-click folders to browse the filesystem
- Press **⌘Space** (or Ctrl+Space) to search
- Open **Terminal** and type `neofetch`, `tree`, or `cat /Desktop/Welcome.txt`
- Play a song in **Music** — it's synthesized live in the browser
- Right-click the desktop · press **F3** for Overview

## Part of the solid-apps desktop family

Glass is a skin/engine in the same family as
[chrome](https://github.com/solid-apps/chrome),
[ubuntu](https://github.com/solid-apps/ubuntu), and
[win98](https://github.com/solid-apps/win98).

The roadmap is to back it with real [Solid](https://solidproject.org) pod plumbing:

1. **VFS → LDP** — the virtual filesystem becomes a write-through cache of your pod's
   containers and resources, kept fresh by WebSocket notifications
2. **Settings → pod** — preferences (accent, wallpaper, blur) sync to your pod instead
   of localStorage
3. **Home skin for [jspod](https://github.com/JavaScriptSolidServer/jspod)** — served
   same-origin from the pod itself, so Files, Notes, and Terminal operate on real data
   with the pod's own auth
4. **Panes** — converge on the shared pane spec so apps are interchangeable across the
   glass, chrome, ubuntu, and win98 skins

## Architecture

Everything lives in `index.html`:

| Module | Role |
|---|---|
| `WM` | window manager — drag, focus/z-order, minimize, zoom, Overview |
| `VFS` | virtual filesystem — `ls/read/write/mkdir/rm/restore/search`, trash semantics |
| `APPS` | app registry — `{ name, icon, w, h, build(body, win, args) }` |
| `Store` | preferences, persisted to localStorage |
| `h()` | tiny hyperscript helper; no framework |

Adding an app is one `build` function and one registry entry.

## License

[AGPL-3.0-only](LICENSE)
