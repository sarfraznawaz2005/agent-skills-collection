---
name: take-screenshots
description: A Windows screenshot skill that provides both programmatic full-screen capture and interactive region selection.
license: ""
compatibility: opencode
---

# take-screenshots

## Purpose

RegionSnip is a Windows screenshot capture utility written in C# that provides both programmatic full-screen capture and interactive region selection.

## Usage

RegionSnip is designed to be called programmatically and outputs JSON results to stdout.

### Command Line Arguments

| Argument | Description | Example |
|----------|-------------|---------|
| `--mode <mode>` | Capture mode: `full` or `region` (default: region) | `--mode full` |
| `--out <path>` | Output PNG file path (required) | `--out screenshot.png` |
| `--all` | Capture all monitors (full mode only) | `--all` |
| `--monitor <n>` | Specific monitor index (0-based, full mode only) | `--monitor 1` |
| `--prompt <text>` | Custom prompt text for region selection | `--prompt "Select area"` |

### Examples

#### Full-screen capture of primary monitor:
```bash
./scripts/RegionSnip.exe --mode full --out screenshot.png
```

#### Full-screen capture of all monitors:
```bash
./scripts/RegionSnip.exe --mode full --out screenshot.png --all
```

#### Full-screen capture of specific monitor:
```bash
./scripts/RegionSnip.exe --mode full --out screenshot.png --monitor 1
```

#### Interactive region selection:
```bash
./scripts/RegionSnip.exe --mode region --out screenshot.png
```

#### Interactive region selection with custom prompt:
```bash
./scripts/RegionSnip.exe --mode region --out screenshot.png --prompt "Drag to select the area to capture"
```

## Output Format

RegionSnip outputs JSON to stdout with the following structure:

### Success Response (Full Mode):
```json
{
  "ok": true,
  "path": "screenshot.png",
  "mode": "full",
  "monitorIndex": 0,
  "all": false,
  "rect": {
    "x": 0,
    "y": 0,
    "width": 1920,
    "height": 1080
  },
  "width": 1920,
  "height": 1080
}
```

### Success Response (Region Mode):
```json
{
  "ok": true,
  "path": "screenshot.png",
  "mode": "region",
  "rect": {
    "x": 100,
    "y": 100,
    "width": 800,
    "height": 600
  },
  "width": 800,
  "height": 600
}
```

### Error Response:
```json
{
  "ok": false,
  "error": "Description of what went wrong",
  "mode": "region"
}
```

### Cancellation Response (Region Mode):
```json
{
  "ok": false,
  "cancelled": true,
  "mode": "region"
}
```

## Error Handling

- Always check the `ok` field in the response
- For region selection, users can cancel the operation (results in `cancelled: true`)
- Common errors include invalid monitor indices, permission issues, or timeout
- Screenshots are saved as PNG files to the specified path
- Base64 image data is included when `includeImage=true` for further processing
