# Zepto Order Automation

An automated FastAPI service that helps you place orders on Zepto quickly using browser automation.

## Features

- 🛒 Add products to your Zepto cart with a simple API call
- 🚀 Handles search, cart management, and payment initiation
- 🔄 Automatically manages browser sessions
- 📱 Support for UPI payments
- 📷 Captures screenshots for verification
- 📝 Detailed logging for debugging
- 🖥️ Cross-platform (macOS and Windows)
- 🔧 n8n workflow integration for automation

## Requirements

- Python 3.8+
- Google Chrome browser
- FastAPI
- Playwright
- n8n (for workflow automation)

## Installation

### Step 1: Clone this repository

```bash
git clone https://github.com/aruntemme/n8n-zepto.git
cd n8n-zepto
```

### Step 2: Install dependencies

```bash
pip install -r requirements.txt
```

### Step 3: Install Playwright browsers

```bash
playwright install chromium
```

## Usage

### Starting the Service

```bash
python main.py
```

This will start the FastAPI server on `http://localhost:8000`.

### Placing an Order

To place an order, send a POST request to the `/order` endpoint:

```bash
curl -X POST http://localhost:8000/order \
  -H "Content-Type: application/json" \
  -d '{
    "products": ["lays", "water bottle"],
    "upi_id": "your.upi@provider"
  }'
```

## n8n Workflow Integration

This project includes pre-configured n8n workflows that you can import to automate your Zepto orders.

For n8n support, you can check out these resources:
- [Official n8n Repository](https://github.com/n8n-io/n8n)
- [ClaraVerse GitHub](https://github.com/badboysm890/ClaraVerse) - A privacy-first AI assistant with n8n-style flow builder
- [ClaraVerse Website](https://claraverse.netlify.app/) - Web interface for the ClaraVerse AI assistant
- [YouTube Tutorial](https://www.youtube.com/watch?v=eoDXeudl1KY) - Detailed explanation of this workflow (in Tamil)

### Scheduled Order Workflow

The scheduled workflow allows you to place Zepto orders at specific times automatically.

1. Import the workflow file:
   - Open your n8n instance
   - Go to Workflows > Import from file
   - Select the `zepto-scheduled-workflow.json` file

2. Configure the workflow:
   - Set your preferred schedule in the "Schedule" node
   - Configure your product list and UPI ID in the "HTTP Request" node

### Voice-Triggered Workflow

The voice workflow allows you to trigger Zepto orders using voice commands via integration with frontend with voice assistant.

1. Import the workflow file:
   - Open your n8n instance
   - Go to Workflows > Import from file
   - Select the `zepto-voice-workflow.json` file

2. Configure the workflow:
   - Connect your preferred voice assistant webhook in the "Webhook" node
   - Set up product mappings in the "Function" node
   - Configure your UPI ID in the "HTTP Request" node

## Platform-Specific Setup

### macOS

1. The script will automatically locate Chrome at:
   ```
   /Applications/Google Chrome.app/Contents/MacOS/Google Chrome
   ```

2. Make sure you have permission to run Chrome. If you're using a managed Mac, you might need to adjust permissions.

3. If you encounter issues with the automated Chrome startup, you can manually start Chrome with:
   ```bash
   /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --user-data-dir=/tmp/zepto_chrome_profile
   ```

### Windows

1. For Windows, you need to modify the Chrome path in `main.py`:

   ```python
   # Change this in the ensure_chrome_running function:
   chrome_path = 'C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe'  # Windows path
   ```

2. Windows commands to check if Chrome is running and to kill Chrome processes:

   ```python
   # For Windows, change these lines
   chrome_running = subprocess.run(['tasklist', '/FI', 'IMAGENAME eq chrome.exe'], capture_output=True, text=True)
   if 'chrome.exe' in chrome_running.stdout.strip():
       # Kill Chrome in Windows
       subprocess.run(['taskkill', '/F', '/IM', 'chrome.exe'])
   ```

3. If you encounter issues with the automated Chrome startup, you can manually start Chrome with:
   ```cmd
   "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir=%TEMP%\zepto_chrome_profile
   ```

## Configuration

You can configure the application by modifying the following parameters in `main.py`:

- `debug_port`: The port used for Chrome debugging (default: 9222)
- Log file location: By default, logs are stored in the current directory with timestamp
- Chrome profile: By default, a temporary Chrome profile is used

## Troubleshooting

### Chrome Won't Start

1. Make sure Chrome is installed in the default location
2. Try manually starting Chrome with the debug flag
3. Check if any other Chrome instances are running
4. Verify port 9222 is not in use by another application

### Payment Issues

1. Make sure your UPI ID is valid
2. Check if you're logged into Zepto
3. Verify that the products are available in your location

### Product Not Found

1. Try using the exact product name from Zepto
2. Check if the product is available in your location
3. Verify that you have access to the search URL

### n8n Workflow Issues

1. Ensure your n8n instance is running and can connect to your FastAPI service
2. Check that the workflow JSON files are correctly imported
3. Verify all credentials and parameters in the workflow nodes

## License

MIT

## Disclaimer

This tool is for educational purposes only. Please use responsibly and adhere to Zepto's terms of service. 