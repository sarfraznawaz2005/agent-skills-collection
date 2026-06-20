---
name: live-browser
description: Drive the user's real, logged-in Chrome / Edge / Brave browser via the Chrome DevTools Protocol — list open tabs, screenshot, read the accessibility tree, evaluate JavaScript, click, type, and navigate on live pages. Unlike headless/automation tools it uses a persistent real profile (logins survive) and sets NO automation flags (no "controlled by automated test software" banner, navigator.webdriver stays false), so sites treat it as an ordinary human browser. Use when the user asks to inspect, automate, scrape, fill, debug, or interact with a page in their browser — especially anything behind a login.
---

# Live Browser

Drive a **real** Chrome / Edge / Brave session over the Chrome DevTools Protocol —
no Puppeteer, instant connection, works with 100+ tabs. The browser runs with a
**dedicated persistent profile** and **no automation flags**, so:

- **No "controlled by automated test software" banner** and `navigator.webdriver`
  stays `false` — pages see an ordinary browser.
- **Logins persist** — sign into your sites once in the launched window; cookies
  and sessions survive across launches.

> Chrome 136+ refuses `--remote-debugging-port` against the *default* profile, so
> this skill drives a separate, dedicated profile window — not your everyday
> browser window. Both can run at once.

## Requirements

- **Node.js 21+** installed (the CDP client uses Node's built-in `WebSocket`/`fetch`).
- Chrome, Edge, or Brave installed.

## Running the scripts

All commands invoke the bundled scripts under this skill's `scripts/` directory
via the `Bash` tool. Paths below are written relative to the skill directory;
resolve them against this skill's base directory (announced when the skill loads),
e.g. `node "<skill-dir>/scripts/launch.mjs"`.

## Step 0 — Start the live browser (run this first)

Before any other command, start the browser. This is idempotent — it's a no-op
if the browser is already up, so it's safe to run at the start of any task. One
command, all platforms:

```bash
node scripts/launch.mjs
```

Pick a specific browser, or open a URL on launch:
```bash
node scripts/launch.mjs --browser edge
node scripts/launch.mjs --url https://example.com
```

`auto` (default) prefers Chrome, then Edge, then Brave. Re-running is a no-op if
the browser is already up. The launcher picks an uncommon free port and
**persists it** (so `cdp.mjs` always finds the right instance — you never pass a
port). **The first time**, log into the sites you care about in that window —
the profile is persistent, so you stay signed in across launches.

> **Timeouts:** the first `launch.mjs` can take ~20s (cold browser start) — well
> within the default `Bash` timeout, so don't set a short timeout for these
> commands. All `cdp.mjs` commands return promptly once the browser is up.

## Commands

All page commands use `node scripts/cdp.mjs`. The `<target>` is a **unique**
targetId prefix from `list`; copy the full prefix shown (for example `6BE827FA`).
Ambiguous prefixes are rejected.

The client reads the launcher's persisted port automatically — you normally
never pass a port. To target a different debug browser, set `CDP_PORT` (or
`CDP_URL` for a full `ws://` endpoint), e.g. `CDP_PORT=9333 node scripts/cdp.mjs list`
(PowerShell: `$env:CDP_PORT=9333`).

### List open pages
```bash
node scripts/cdp.mjs list
```
Run this first — it discovers target prefixes and caches them for other commands.

### Take a screenshot
```bash
node scripts/cdp.mjs shot <target> [file]
# default: screenshot-<target>.png in the runtime dir
```
Captures the **viewport only**. Scroll first with `eval` for content below the
fold. Output includes the page DPR and a coordinate-conversion hint (see
**Coordinates**). Read the saved PNG to view it.

### Accessibility tree snapshot
```bash
node scripts/cdp.mjs snap <target>
```
Prefer `snap` over `html` for structure — it is faster and token-efficient.

### Evaluate JavaScript
```bash
node scripts/cdp.mjs eval <target> <expr>
```
> **Watch out:** avoid index-based selection (`querySelectorAll(...)[i]`) across
> multiple `eval` calls when the DOM can change between them (indices shift after
> clicks). Collect all data in one `eval` or use stable selectors.

### Other commands
```bash
node scripts/cdp.mjs html    <target> [selector]   # full page or element HTML
node scripts/cdp.mjs nav     <target> <url>         # navigate and wait for load
node scripts/cdp.mjs net     <target>               # resource timing entries
node scripts/cdp.mjs click   <target> <selector>    # click element by CSS selector
node scripts/cdp.mjs clickxy <target> <x> <y>       # click at CSS pixel coords
node scripts/cdp.mjs type    <target> <text>         # Input.insertText at focus; works in cross-origin iframes unlike eval
node scripts/cdp.mjs loadall <target> <selector> [ms]  # click "load more" until gone (default 1500ms between clicks)
node scripts/cdp.mjs evalraw <target> <method> [json]  # raw CDP command passthrough
node scripts/cdp.mjs open    [url]                  # open a new tab
node scripts/cdp.mjs stop    [target]               # stop daemon(s)
```

## Coordinates

`shot` saves an image at native resolution: image pixels = CSS pixels × DPR.
CDP Input events (`clickxy` etc.) take **CSS pixels**.

```
CSS px = screenshot image px / DPR
```

`shot` prints the DPR for the current page. Typical Retina (DPR=2): divide
screenshot coords by 2.

## Tips

- Run `list` first to discover targetId prefixes before any other command.
- Use `type` (not `eval`) to enter text in cross-origin iframes — `click`/`clickxy`
  to focus first, then `type`.
- A background daemon holds each tab's CDP session open so repeated commands are
  instant; daemons auto-exit after 20 minutes of inactivity.
- This drives a real, persistent profile — treat the user's logged-in sessions
  with care and only perform actions the user asked for.
