# ❖ Glass

**A liquid-glass desktop for your Solid pod — one HTML file, no build step.**

🔗 **Live demo:** https://solid-apps.github.io/glass/

Glass is a self-contained desktop environment in a single `index.html`: a real
window manager (drag, minimize-to-dock, zoom, Overview), menu bar, dock with
magnification, ⌘Space search, notifications, Control Center, and a suite of
built-in apps — Files, Compass (browser), Notes, Terminal, Music (real-time
WebAudio synth), Photos, Weather, Clock, Calculator, Registry, Settings, Trash.

Open the file. That's the install.

## Solid pod integration

Sign in from the menu bar (**Sign In** → Solid OIDC or Nostr, via
[xlogin](https://github.com/melvincarvalho/xlogin)) and your pod mounts at
`/Pod`:

- **Files** browses your pod's LDP containers next to the local folders —
  lazily hydrated per-container, with live repaints as listings arrive
- **Writes go through**: save a note, `mkdir`, `rm` — the cache updates
  instantly and PUT/DELETE hit the pod in the background
- **Notes** edits pod text resources with debounced autosave back to the pod
- **Terminal** speaks pod: `login`, `whoami` (your WebID), `pod`,
  `cat /Pod/…`, and `mount <url>` to mount any public LDP container without
  signing in
- **Live updates**: solid-0.1 `Updates-Via` WebSocket subscriptions re-sync
  containers when another client writes

The VFS stays synchronous — the pod is a write-through cache, so every app
works unchanged whether a file is local or remote.

## SLIP-48 panes & the solid-apps registry

Glass shares the pane/app ecosystem of
[hub](https://github.com/solid-apps/hub),
[chrome](https://github.com/solid-apps/chrome),
[ubuntu](https://github.com/solid-apps/ubuntu), and
[win98](https://github.com/solid-apps/win98):

- Open a JSON-LD resource on your pod and Glass matches its `rdf:type`
  against the [registry](https://github.com/solid-apps/registry) and renders
  it with the same SLIP-48 pane module those shells use
  (`render(subject, store, container, rawData, ctx)`)
- The **Registry** app in the dock lists community apps
  (`render(container, ctx)`) and runs them in Glass windows
- Served from solid-apps.github.io, Glass honours hub's pinned per-class
  pane defaults (same-origin localStorage)

## Things to try

- Open **Terminal**: `neofetch`, `tree`, `mount solidcommunity.net` …
- Press **⌘Space** to search · **F3** for Overview
- Open **Registry** from the dock and launch a community app
- Sign in, then browse `/Pod` in Files — open a `.jsonld` note and watch it
  render in the shared note pane

## Architecture

Everything lives in `index.html`:

| Module | Role |
|---|---|
| `WM` | window manager — drag, focus/z-order, minimize, zoom, Overview |
| `VFS` | synchronous virtual filesystem with trash semantics |
| `Pod` | LDP write-through cache mounted at `/Pod` + Updates-Via WebSockets |
| `Auth` | reactive wrapper over the xlogin widget (Solid OIDC / Nostr) |
| `Panes` | solid-apps registry loader + SLIP-48 pane dispatch |
| `APPS` | app registry — `{ name, icon, w, h, build(body, win, args) }` |
| `h()` | tiny hyperscript helper; no framework |

External at runtime (all optional, Glass works fully offline without them):
`unpkg.com/xlogin` for auth, `solid-apps.github.io/registry` for panes/apps.

## Roadmap

- Serve same-origin as the home skin for
  [jspod](https://github.com/JavaScriptSolidServer/jspod) so auth rides the
  pod's own session
- Publish Glass's built-in apps as registry panes so they run in the other
  shells too

## License

[AGPL-3.0-only](LICENSE)
