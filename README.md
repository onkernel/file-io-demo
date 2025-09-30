# Kernel File I/O Demo

A demonstration of file download automation using [Kernel](https://onkernel.com) cloud browsers with Playwright and Chrome DevTools Protocol (CDP).

This example shows how to automate downloading files from websites using Kernel's remote browser infrastructure, then retrieve those files from the cloud browser's filesystem to your local machine.
[![Kernel File I/O Youtube Video Screenshot](https://github.com/user-attachments/assets/44e168e6-5392-4002-85fc-59618684b92b)](https://www.youtube.com/watch?v=zJWSa-Eqbfs)

## What This Demo Does

This script automates a receipt download workflow:

1. Creates a remote browser session via Kernel
2. Navigates to a parking receipt lookup page
3. Fills out a form with email and order number
4. Downloads a PDF receipt
5. Retrieves the file from the remote browser filesystem
6. Streams it to your local `./downloads/` directory

## Architecture

The demo combines three key technologies:

- **Kernel**: Provides cloud-hosted browsers accessible via API
- **Playwright**: Automates browser interactions (form filling, clicking, navigation)
- **CDP (Chrome DevTools Protocol)**: Handles file download tracking and retrieval

### Key Technical Highlights

**File Download Handling via CDP**

Unlike standard Playwright download handling, this demo uses CDP to:
- Set download behavior on the remote browser (`Browser.setDownloadBehavior`)
- Track download events (`Browser.downloadWillBegin`, `Browser.downloadProgress`)
- Access files in the remote filesystem via Kernel's file API

**Streaming File Transfer**

Files are streamed from the remote browser to local disk using `client.browsers.fs.read_file()` and `write_to_file()`, avoiding loading large files into memory.

## Prerequisites

- Python 3.11 or higher
- [uv](https://docs.astral.sh/uv/) package manager
- A Kernel API key (sign up at [onkernel.com](https://onkernel.com))

## Installation

1. Clone this repository:
```bash
git clone <your-repo-url>
cd file-io-demo
```

2. Install dependencies using uv:
```bash
uv sync
```

3. Create a `.env` file in the project root:
```bash
KERNEL_API_KEY=your_api_key_here
OPENAI_API_KEY=your_openai_api_key_here
```

## Usage

Run the automation script:

```bash
python main.py
```

The script will:
- Print the Kernel browser live view URL (watch the automation in real-time)
- Navigate through the demo site
- Download the receipt
- Save it to `./downloads/`

### Customizing for Your Use Case

To adapt this for your own website automation:

1. Update the configuration variables in `main.py`:
   ```python
   WEBSITE_URL = "your-website-url"
   EMAIL = "your-email"
   ORDER_NUMBER = "your-order-number"
   DOWNLOAD_DIR = "/tmp/downloads"
   ```

2. Modify the Playwright selectors to match your target website's HTML structure

3. Adjust the CDP event handlers if needed for different download patterns

## Project Structure

- **`main.py`** - Main automation script with download logic
- **`session.py`** - Custom `BrowserSession` subclass that fixes viewport resizing when connecting via CDP
- **`pyproject.toml`** - Project dependencies and configuration
- **`.env`** - Environment variables (API keys)

## Development

Type checking:
```bash
mypy .
```

## Learn More

- [Kernel Documentation](https://docs.onkernel.com)
- [Kernel Python SDK](https://github.com/onkernel/kernel-python-sdk)
- [Playwright Python](https://playwright.dev/python/)
- [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)

## License

MIT
