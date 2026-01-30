---
name: neutralinojs
description: Lightweight cross-platform desktop application framework for JavaScript, HTML, and CSS. Provides native OS operations, window management, filesystem access, and extensibility via extensions. Alternative to Electron with minimal bundle size.
references:
  - api
  - cli
  - configuration
  - how-to
  - getting-started
---

# Neutralino.js Platform Skill

Consolidated skill for building cross-platform desktop applications with Neutralino.js. Use decision trees below to find the right APIs and configuration options, then load detailed references.

## Critical Rules

**Follow these rules in all Neutralino.js code:**

1. **Always use `neu create <path>` for new projects** unless explicitly asked to create from scratch.
2. **Import `@neutralinojs/lib` for frontend frameworks** (React, Vue, etc.) instead of including `neutralino.js` directly.
3. **Never use `tokenSecurity: none`** when exposing native APIs - always use `one-time` (default) for security.
4. **Configure `nativeAllowList` and `nativeBlockList`** to restrict API access and enhance security.
5. **Call `Neutralino.init()`** before using any native API methods.
6. **Handle window close events** properly using `events.on('windowClose')` - never rely solely on the close button.

## How to Use This Skill

### Reference File Structure

Neutralino.js references follow a hierarchical structure. Cross-cutting concepts and how-to guides are separate.

**API References** in `./references/api/`:
- `overview.md` - All API namespaces and architecture
- `app.md` - Application lifecycle, config, broadcasting
- `window.md` - Window management (create, focus, size, tray)
- `filesystem.md` - File operations, watchers, paths
- `os.md` - OS operations, environment variables, spawning processes
- `storage.md` - Key-value storage (SQLite-based)
- `clipboard.md` - System clipboard access
- `computer.md` - System info (memory, OS version)
- `events.md` - Event system (on, off, dispatch, broadcast)
- `extensions.md` - Extension communication
- `custom.md` - Custom method execution
- `resources.md` - Bundle resource access
- `server.md` - Server info and configuration
- `updater.md` - Auto-updater functionality
- `init.md` - Initialization API
- `error-codes.md` - Error code reference
- `global-variables.md` - Predefined global variables

**CLI Reference** in `./references/cli/`:
- `neu-cli.md` - All CLI commands (create, run, build, update, etc.)
- `internal-cli-arguments.md` - Framework binary CLI arguments

**Configuration** in `./references/configuration/`:
- `neutralino.config.json.md` - Complete config reference
- `modes.md` - Application modes (window, browser, cloud, chrome)
- `project-structure.md` - Project directory layout

**Getting Started** in `./references/getting-started/`:
- `introduction.md` - What is Neutralino.js, features, comparisons
- `using-frontend-libraries.md` - React, Vue, Angular integration

**How-To Guides** in `./references/how-to/`:
- `extensions-overview.md` - Building and using extensions
- `auto-updater.md` - Implementing auto-updates

**Distribution** in `./references/distribution/`:
- `overview.md` - Building and distributing applications

### Reading Order

1. **Start with** `getting-started/introduction.md` for overview
2. **Then read** based on your task:
   - Creating new app -> `cli/neu-cli.md` (neu create)
   - Using native APIs -> `api/overview.md` + specific namespace
   - Window management -> `api/window.md` + `configuration/modes.md`
   - File operations -> `api/filesystem.md`
   - Configuration -> `configuration/neutralino.config.json.md`
   - Building extensions -> `how-to/extensions-overview.md`
   - Auto-updater -> `how-to/auto-updater.md`
   - Frontend integration -> `getting-started/using-frontend-libraries.md`
   - Distribution -> `distribution/overview.md`

### Example Paths

```
./references/getting-started/introduction.md       # Start here
./references/cli/neu-cli.md                         # CLI commands
./references/api/overview.md                        # All APIs overview
./references/api/filesystem.md                       # File operations
./references/configuration/neutralino.config.json.md # Config reference
./references/how-to/extensions-overview.md          # Extensions
./references/api/window.md                          # Window management
```

## Quick Decision Trees

### "I want to create a new Neutralino.js app"

```
Create app?
├─ Quick start with default template -> neu create myapp
├─ Use custom template -> neu create myapp --template=<account>/<repo>
├─ Integrate with frontend framework -> getting-started/using-frontend-libraries.md
└─ From scratch (expert) -> configuration/neutralino.config.json.md
```

### "I need to manage windows"

```
Window management?
├─ Create/show window -> api/window.md (window.create, window.show)
├─ Set window size/position -> api/window.md (window.setSize, window.setPosition)
├─ Window focus/maximize/minimize -> api/window.md (window.focus, window.maximize, window.minimize)
├─ System tray icon -> api/window.md (window.setTrayOptions)
├─ Transparent window -> configuration/neutralino.config.json.md (modes.window.transparent)
├─ Borderless window -> configuration/neutralino.config.json.md (modes.window.borderless)
└─ Fullscreen mode -> api/window.md (window.enterFullScreen)
```

### "I need to work with files"

```
File operations?
├─ Read text file -> api/filesystem.md (readFile)
├─ Write text file -> api/filesystem.md (writeFile)
├─ Read directory -> api/filesystem.md (readDirectory)
├─ Create/remove directory -> api/filesystem.md (createDirectory, remove)
├─ Copy/move files -> api/filesystem.md (copy, move)
├─ Watch file changes -> api/filesystem.md (createWatcher)
├─ File stats (exists, size, modified) -> api/filesystem.md (getStats)
├─ Path operations -> api/filesystem.md (getAbsolutePath, getRelativePath)
└─ File permissions -> api/filesystem.md (getPermissions, setPermissions)
```

### "I need OS-level operations"

```
OS operations?
├─ Execute system command -> api/os.md (execCommand)
├─ Get environment variable -> api/os.md (getEnv)
├─ Set environment variable -> api/os.md (setEnv)
├─ Spawn process with I/O -> api/os.md (spawnProcess)
├─ Get OS info -> api/computer.md (getOsInfo)
├─ Get memory info -> api/computer.md (getMemoryInfo)
├─ Open external URL -> api/os.md (open)
└─ Path constants (NL_PATH, NL_CWD) -> api/global-variables.md
```

### "I need data storage"

```
Storage?
├─ Simple key-value storage -> api/storage.md (getData, setData, putData)
├─ Persist data across app restarts -> api/storage.md
└─ Custom database -> how-to/extensions-overview.md (build extension)
```

### "I need to handle events"

```
Events?
├─ Window close event -> api/events.md (windowClose)
├─ Window focus/blur -> api/events.md (windowFocus, windowBlur)
├─ Tray menu click -> api/events.md (trayMenuItemClicked)
├─ App instance events -> api/events.md (appClientConnect, appClientDisconnect)
├─ Extension events -> api/events.md (extensionReady, extClientConnect)
└─ Custom events -> api/events.md (dispatch, broadcast)
```

### "I need to build and distribute"

```
Build & distribute?
├─ Build for current platform -> cli/neu-cli.md (neu build)
├─ Create portable ZIP -> cli/neu-cli.md (--release)
├─ Single-file executable -> cli/neu-cli.md (--embed-resources)
├─ Auto-updater -> how-to/auto-updater.md
└─ Distribution overview -> distribution/overview.md
```

### "I need to use extensions"

```
Extensions?
├─ Overview and concepts -> how-to/extensions-overview.md
├─ Define extensions in config -> configuration/neutralino.config.json.md (extensions)
├─ Enable extensions -> configuration/neutralino.config.json.md (enableExtensions)
├─ Dispatch to extension -> api/extensions.md (dispatch)
├─ Extension receives messages -> how-to/extensions-overview.md (WebSocket protocol)
└─ Extension calls native API -> how-to/extensions-overview.md (app.broadcast)
```

### "I need app lifecycle control"

```
Lifecycle?
├─ Exit app -> api/app.md (exit)
├─ Kill process (force) -> api/app.md (killProcess)
├─ Restart app -> api/app.md (restartProcess)
├─ Get app config -> api/app.md (getConfig)
├─ Broadcast to all instances -> api/app.md (broadcast)
├─ Read/write stdin/stdout -> api/app.md (readProcessInput, writeProcessOutput)
└─ Initialize client -> api/init.md (init)
```

### "Which mode should I use?"

```
Application mode?
├─ Desktop app with native window -> window mode (default)
├─ Web app with native features -> browser mode
├─ Background server/API -> cloud mode
├─ Chrome app style -> chrome mode
└─ Details -> configuration/modes.md
```

### "I need security configuration"

```
Security?
├─ Restrict API access -> configuration/neutralino.config.json.md (nativeAllowList, nativeBlockList)
├─ Token security -> configuration/neutralino.config.json.md (tokenSecurity)
├─ Export auth info -> configuration/neutralino.config.json.md (exportAuthInfo)
└─ Global variables -> api/global-variables.md
```

### "I need to integrate with frontend frameworks"

```
Frontend integration?
├─ React/Vue/Angular -> getting-started/using-frontend-libraries.md
├─ Import client lib -> npm: @neutralinojs/lib
├─ CLI frontend support -> configuration/neutralino.config.json.md (cli.frontendLibrary)
└─ HMR/DevTools -> cli/neu-cli.md (neu run with dev server)
```

## Product Index

### API Namespaces
| Namespace | Entry File | Description |
|-----------|------------|-------------|
| app | `./references/api/app.md` | Application lifecycle, config, broadcasting |
| window | `./references/api/window.md` | Window creation, management, tray |
| filesystem | `./references/api/filesystem.md` | File operations, watchers, paths |
| os | `./references/api/os.md` | OS operations, environment, processes |
| storage | `./references/api/storage.md` | Key-value storage |
| clipboard | `./references/api/clipboard.md` | Clipboard access |
| computer | `./references/api/computer.md` | System info (memory, OS) |
| events | `./references/api/events.md` | Event system |
| extensions | `./references/api/extensions.md` | Extension communication |
| custom | `./references/api/custom.md` | Custom method execution |
| resources | `./references/api/resources.md` | Bundle resources |
| server | `./references/api/server.md` | Server info |
| updater | `./references/api/updater.md` | Auto-updater |
| init | `./references/api/init.md` | Initialization |
| error-codes | `./references/api/error-codes.md` | Error codes |
| global-variables | `./references/api/global-variables.md` | Predefined globals |

### Configuration
| Concept | Entry File | Description |
|---------|------------|-------------|
| Config Reference | `./references/configuration/neutralino.config.json.md` | Complete configuration options |
| Modes | `./references/configuration/modes.md` | Application modes (window, browser, cloud, chrome) |
| Project Structure | `./references/configuration/project-structure.md` | Directory layout |

### CLI
| Concept | Entry File | Description |
|---------|------------|-------------|
| neu CLI | `./references/cli/neu-cli.md` | All CLI commands |
| Internal Arguments | `./references/cli/internal-cli-arguments.md` | Framework binary arguments |

### Getting Started
| Concept | Entry File | Description |
|---------|------------|-------------|
| Introduction | `./references/getting-started/introduction.md` | Overview, features, comparisons |
| Frontend Libraries | `./references/getting-started/using-frontend-libraries.md` | React, Vue, Angular integration |

### How-To
| Concept | Entry File | Description |
|---------|------------|-------------|
| Extensions | `./references/how-to/extensions-overview.md` | Building and using extensions |
| Auto Updater | `./references/how-to/auto-updater.md` | Implementing updates |

### Distribution
| Concept | Entry File | Description |
|---------|------------|-------------|
| Distribution | `./references/distribution/overview.md` | Building and distributing apps |

## Resources

**Repository**: https://github.com/neutralinojs/neutralinojs
**Documentation**: https://neutralino.js.org
**Client Library (NPM)**: https://www.npmjs.com/package/@neutralinojs/lib
**CLI (NPM)**: https://www.npmjs.com/package/@neutralinojs/neu
**Discussions**: https://github.com/neutralinojs/neutralinojs/discussions
