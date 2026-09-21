# MapHarvest — macOS Setup & Troubleshooting Guide

This guide walks you through setting up and running MapHarvest on macOS (Intel and Apple Silicon M1 / M2 / M3 / M4).

---

## 1. Prerequisites

1. **Google Chrome**:
   - Install Google Chrome from [https://www.google.com/chrome](https://www.google.com/chrome).
   - Ensure it is located in your `/Applications/` folder (`/Applications/Google Chrome.app`).

2. **Python 3.10, 3.11, or 3.12**:
   - Check if you have Python 3 installed:
     ```bash
     python3 --version
     ```
   - If not installed, install it using [Homebrew](https://brew.sh/):
     ```bash
     brew install python
     ```

---

## 2. Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/farixdev/MapHarvest.git
   cd MapHarvest
   ```

2. **Create a virtual environment**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies**:
   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

   > **Note for Apple Silicon (M1/M2/M3/M4) users**:
   > If you encounter any issue installing PyQt5 via pip, you can install it via Homebrew:
   > ```bash
   > brew install pyqt@5
   > ```

---

## 3. Running MapHarvest

With the virtual environment activated:
```bash
python main.py
```

---

## 4. macOS-Specific Troubleshooting

### Issue 1: "chromedriver cannot be opened because the developer cannot be verified"
macOS Gatekeeper blocks unsigned binaries downloaded by `undetected-chromedriver`.

**Fix**: Run this command in Terminal to clear the quarantine flag:
```bash
xattr -cr ~/Library/Application\ Support/undetected_chromedriver
```
Then restart the scrape.

---

### Issue 2: Google Chrome Not Found
MapHarvest searches for Chrome at:
- `/Applications/Google Chrome.app`
- `~/Applications/Google Chrome.app`

If Chrome is installed elsewhere, move it into `/Applications/`.

---

### Issue 3: Permission Denied / OS Error on Driver Launch
If `undetected-chromedriver` has a cached binary with broken permissions, delete its cache folder:
```bash
rm -rf ~/Library/Application\ Support/undetected_chromedriver
```
MapHarvest will automatically re-download and patch a fresh compatible driver on the next scrape.

---

## 5. One-Click macOS Runner Script (Optional)

You can create a clickable `run.command` file in the project folder:
```bash
cat << 'EOF' > run.command
#!/bin/bash
cd "$(dirname "$0")"
source venv/bin/activate
python main.py
EOF
chmod +x run.command
```
Now double-clicking `run.command` in Finder will launch MapHarvest directly.
