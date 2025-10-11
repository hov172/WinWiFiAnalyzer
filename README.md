# Windows Wi‑Fi Analyzer 
Wifi report for users to run and send to Support team analyst. 

- Location Services may need to be enabled for accurate results (channel/band/signal). When prompted, choose Allow.

I create a PS (Powershell Script) and a Macos App that does something similar)
- Powershell Script: https://github.com/hov172/PS_WI-FI_Analyzer
- Macos: https://github.com/hov172/WifDiagReport

## Command‑Line (CLI)
Wi‑Fi Analyzer also works headless from the command line on macOS, Windows, and Linux. This is intended for IT automation and scripted diagnostics.

Usage
```
WifiAnalyzer --cli [options]
```

Options
- `--out <path>`: Write output to a file. Use `.pdf` or `.csv` extension, or combine with `--csv`.
- `--csv`: Force CSV export (overrides extension).
- `--name <full name>`: Include user info in PDF.
- `--id <id>`: Include ID number in PDF.
- `--email <email>`: Include email in PDF.
- `--building <name>`: Include building in PDF.
- `--room <room>`: Include room in PDF.
- `--speed`: Run speed test before exporting.
- `--deep`: Deepen scan (multiple passes merged for stronger readings).
- `-h | --help`: Show help.

Examples (Windows PowerShell)
```
# Human‑readable report to console
./WifiAnalyzer.exe --cli

# Export a PDF with user info and speed test
./WifiAnalyzer.exe --cli --speed \
  --name "Jane Doe" --email jane@example.com \
  --out "C:\\Temp\\WiFiReport.pdf"

# Export a CSV of networks
./WifiAnalyzer.exe --cli --csv --out "C:\\Temp\\Networks.csv"

# Deeper scanning (three passes merged)
./WifiAnalyzer.exe --cli --deep --out "C:\\Temp\\WiFiReport.pdf"
```

Notes
- On Windows, `--admin-scan` is ignored. A console is attached automatically when `--cli` is used so output appears in the terminal.
- Exit codes are non‑zero on error, suitable for scripting.


## Windows One-File Installer (CI)
- A GitHub Actions workflow builds a single installer `.exe` that bundles the app and installs VC++ if needed.
- Trigger manually from the Actions tab (workflow "Windows Installer") or by pushing a tag like `v1.0.0`.
- Artifacts: `WifiAnalyzer-Setup-x64.exe` and `WifiAnalyzer-Setup-arm64.exe` (download from the workflow run).

## App Info & Icon
- Name: Wi‑Fi Analyzer
- Version: 2.0
- Publisher: Jesus Ayala from: Ayala Solutions
- Website: https://github.com/hov172
