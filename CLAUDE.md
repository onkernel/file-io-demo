# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Kernel browser automation demo that downloads receipts from a parking lookup website. The app uses Kernel's cloud browser infrastructure with Playwright for automation and CDP (Chrome DevTools Protocol) for file operations.

## Key Architecture

### Main Flow ([main.py](main.py))
1. Creates a remote Kernel browser session via `AsyncKernel()`
2. Connects Playwright to the Kernel browser using CDP WebSocket URL
3. Configures download behavior via CDP session (`Browser.setDownloadBehavior`)
4. Uses CDP event listeners (`Browser.downloadWillBegin`, `Browser.downloadProgress`) to track downloads
5. Retrieves downloaded files from the remote browser filesystem using `client.browsers.fs.read_file()`
6. Streams file to local disk without loading into memory

### Custom Browser Session ([session.py](session.py))
- `BrowserSessionCustomResize`: Overrides `browser-use`'s `_setup_viewports` method
- Fixes viewport resizing issues when connecting via CDP
- Hardcoded viewport: 1024x786
- Handles browser profile settings (permissions, timeouts, headers, geolocation)

## Common Commands

### Run the application
```bash
python main.py
```

### Type checking
```bash
mypy .
```

### Install dependencies
Uses `uv` for dependency management:
```bash
uv sync
```

## Environment Variables

Requires `.env` file (loaded via `python-dotenv`). The Kernel API key should be configured.

## Important Implementation Details

- **File Downloads**: Must use CDP `Browser.setDownloadBehavior` to prevent Playwright from overriding download paths
- **File Retrieval**: Use `client.browsers.fs.read_file()` to access files from remote Kernel browser filesystem
- **Streaming**: File writing uses `resp.write_to_file()` which streams content to avoid memory issues
- **CDP Events**: Download tracking relies on CDP events, not Playwright's download API
- **Session Cleanup**: Always delete Kernel browser session with `client.browsers.delete_by_id()` when done
