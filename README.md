# 📡 Wi-Fi Analyzer

**Cross-platform Wi-Fi analysis, diagnostics, channel intelligence, and support-ready reporting for Windows, macOS, and Linux.**

Wi-Fi Analyzer is a cross-platform desktop application designed to examine the local wireless environment, evaluate connection health, identify potential Wi-Fi problems, and generate structured reports for IT support.

Built with **Avalonia and .NET 8**, the application provides a consistent diagnostic workflow across Windows, macOS, and Linux while using the best Wi-Fi information available from each operating system.

**Current Version: 2.5.3**

![Networks tab showing the scan results table, filters, and per-band congestion charts](images/scan_Main-Networks.png)

---

# 🎯 Purpose

Wi-Fi Analyzer is designed for:

* IT professionals
* Help Desk technicians
* Desktop support teams
* Network administrators
* Students and end users
* Power users
* Remote troubleshooting
* Wi-Fi deployment validation

The application combines **Wi-Fi scanning, radio analysis, connection-health scoring, network performance testing, channel recommendations, system information, privacy-aware reporting, and command-line automation** into a single cross-platform tool.

The goal is not simply to display nearby SSIDs.

Wi-Fi Analyzer attempts to answer the questions an IT technician actually needs answered:

* What access point is the device connected to?
* How strong is the connection?
* Is the channel congested?
* Is another channel measurably better?
* What Wi-Fi generation is the AP advertising?
* What physical link rate was negotiated?
* Is the device seeing better roaming candidates?
* Is DFS being used?
* What roaming capabilities are advertised?
* Is performance consistent with the radio conditions?
* Which measurements came directly from the operating system?
* Which values were derived?
* Which measurements were unavailable?
* Can the results be safely sent to IT?

---

# 🌐 Wi-Fi Analyzer Project Family

Wi-Fi Analyzer is part of a larger collection of Wi-Fi diagnostic and troubleshooting tools I have developed across multiple platforms and technologies.

| Platform                       | Project                             | Access                                                                                                             |
| ------------------------------ | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| 🪟 **Windows / macOS / Linux** | Wi-Fi Analyzer                      | This Project                                                                                                       |
| 🖥️ **PowerShell**             | PS Wi-Fi Analyzer                   | [GitHub](https://github.com/hov172/PS_WI-FI_Analyzer)                                                              |
| 🍎 **macOS**                   | Wi-Fi Analyzer — Windows-style UI   | [Apple silicon DMG](https://github.com/hov172/WinWiFiAnalyzer/releases/latest/download/Wi.Fi.Analyzer.dmg) · [Intel DMG](https://github.com/hov172/WinWiFiAnalyzer/releases/latest/download/Wi.Fi.Analyzer-Intel.dmg) |
| 📱 **iOS / iPadOS**            | Wi-Fi Analyzer Beta                 | [Apple TestFlight](https://testflight.apple.com/join/hDPGsHzd)                                                     |
| 🍎 **macOS Advanced**          | WiFi Diag Report + Coverage Mapping | [GitHub](https://github.com/hov172/WifDiagReport)                                                                  |

Different implementations exist because Windows, macOS, Linux, and iOS expose different wireless information and networking APIs.

The common goal is:

> **Collect useful Wi-Fi and network information, identify potential connectivity problems, and provide IT support teams with actionable diagnostic data.**

---

# 🚀 Core Features

## 📡 Wi-Fi Environment Scanner

Scan nearby wireless networks across:

* 2.4 GHz
* 5 GHz
* Observed 6 GHz networks

Collected information can include:

* SSID
* BSSID
* RSSI
* Signal quality
* Channel
* Channel width
* Frequency
* Security
* Wi-Fi band
* Connected AP status
* Wi-Fi generation
* Channel utilization
* Roaming capabilities

The application identifies the currently connected access point when the operating system provides enough information to determine its BSSID.

---

# 📊 Radio Engineering Detail

Wi-Fi Analyzer goes beyond basic SSID and signal-strength scanning.

Depending on platform capabilities, the application can analyze:

### PHY Link Rate

Reports negotiated transmit and receive PHY rates where available.

PHY rate is intentionally kept separate from application-level throughput so the two measurements are not confused.

### Wi-Fi Standard

Access-point capabilities can identify generations such as:

* Wi-Fi 4
* Wi-Fi 5
* Wi-Fi 6
* Wi-Fi 6E
* Wi-Fi 7

### Noise & SNR

Where the operating system exposes them, Wi-Fi Analyzer reports:

* Noise floor
* Signal-to-noise ratio

If the platform cannot provide these values, they are reported as **unmeasured** rather than estimated as real measurements.

### MCS

Modulation and Coding Scheme information is displayed when reported or when it can be derived from available link information.

### Spatial Streams

Spatial-stream information helps describe the MIMO configuration being used by the connection.

### Channel Width

Channel-width information can help distinguish connections operating at widths such as:

* 20 MHz
* 40 MHz
* 80 MHz
* 160 MHz

### Guard Interval

Guard-interval information is displayed where available or derivable.

---

# 📶 Channel Utilization & Congestion

Wi-Fi Analyzer evaluates wireless congestion using more than the number of visible networks.

Where beacon information is available, it can decode the **BSS Load** element and report measured channel utilization.

This represents the percentage of airtime that the access point reports as busy.

That information can be considered alongside:

* Nearby AP count
* RSSI
* Channel overlap
* Band
* Channel width
* Measured utilization

This provides better evidence for channel recommendations than simply counting nearby SSIDs.

---

# 🧠 Intelligent Channel Recommendations

Wi-Fi Analyzer evaluates the measured wireless environment and recommends channels based on actual scan evidence.

Recommendations include:

* Recommended channel
* Band
* Congestion evidence
* Signal overlap
* Confidence
* Plain-language rationale

This helps technicians understand **why** a channel is being recommended rather than receiving an unexplained channel number.

---

# 🛰️ Roaming Analysis

For the connected SSID, Wi-Fi Analyzer can identify potential roaming candidates.

The analysis can include:

* Candidate AP
* RSSI
* Signal difference from the current AP
* Connected AP comparison
* Advertised 802.11k support
* Advertised 802.11v support
* Advertised 802.11r support

This can help identify environments where a client remains associated with a weaker AP even though stronger candidates are visible.

---

# 🌦️ DFS Analysis

Wi-Fi Analyzer identifies access points operating on DFS channels.

Rather than simply flagging DFS, the application explains the potential trade-offs and evaluates whether measured congestion may justify its use.

This gives technicians additional context when evaluating:

* Channel selection
* Radar-sensitive channels
* Congestion
* Available alternatives

---

# ❤️ Connection Health Score

Wi-Fi Analyzer calculates an overall connection-health score from **0–100**.

The score evaluates multiple diagnostic areas:

* 📶 Signal
* 🚀 Speed
* 📡 Congestion
* 🔐 Security
* 🛡️ Reliability

The application shows the measurement behind each scoring axis.

### Honest Handling of Missing Data

Unavailable measurements are **not assumed to be good**.

If a value cannot be measured:

1. It is marked as **unmeasured**.
2. It is excluded from the health weighting.
3. Remaining measurements are reweighted appropriately.

If less than half of the normal health weighting is available, the resulting score is marked:

**Provisional**

This prevents missing measurements from artificially improving a connection-health score.

---

# 💻 System & Device Information

Each scan captures support-relevant computer information.

This can include:

* Computer model
* Operating system
* OS version
* OS build
* Architecture
* Hostname
* IP address
* Wi-Fi interface address

This information is incorporated into support reports where appropriate.

---

# 🌐 Network Performance Testing

The graphical application can automatically perform a network performance test following a scan.

Measurements can include:

* Download throughput
* Upload throughput
* Latency
* Jitter
* Packet loss

Deep scanning and speed testing are enabled by default in the graphical application.

The command-line interface behaves differently by design: network performance testing is opt-in so an automated/headless scan does not transfer network-test data unless explicitly requested.

---

# 🖥️ Supported Platforms

| Platform          | Wi-Fi Scanner                                       | Requirements                                            |
| ----------------- | --------------------------------------------------- | ------------------------------------------------------- |
| **Windows 10/11** | Native Wi-Fi API with `netsh` fallback              | WLAN AutoConfig, enabled Wi-Fi adapter, location access |
| **macOS 11+**     | CoreWLAN with structured `system_profiler` fallback | Wi-Fi and Location Services                             |
| **Linux**         | `nmcli` with `iw` for additional link detail        | NetworkManager and permission to query Wi-Fi            |

The diagnostic workflow remains consistent across operating systems while controls, fonts, window chrome, and permission prompts follow the native host environment.

---

# 🔬 Measurement Sources by Platform

Wi-Fi capabilities vary significantly between operating systems.

Wi-Fi Analyzer therefore distinguishes between:

* **Reported values** — supplied directly by the operating system/driver
* **Derived values** — calculated from other available measurements
* **Unmeasured values** — unavailable from the current platform

| Detail                         | Windows                | macOS                              | Linux                              |
| ------------------------------ | ---------------------- | ---------------------------------- | ---------------------------------- |
| PHY link rate                  | Reported TX/RX         | Reported TX                        | Reported TX/RX                     |
| Noise floor / SNR              | Not exposed by OS      | Reported                           | Reported when driver publishes it  |
| MCS / Streams / Guard Interval | Derived from link rate | Derived from link rate             | Reported by `iw`                   |
| Wi-Fi Standard                 | Beacon decoding        | Reported/available platform detail | Reported/available platform detail |
| Channel Utilization            | Beacon decoding        | Limited by platform                | Limited by platform                |
| 802.11k/v/r                    | Beacon decoding        | Limited                            | Limited                            |

The report identifies the source so a derived value is never presented as though it were a direct driver measurement.

---

# ⚡ Using Wi-Fi Analyzer

## Basic Workflow

1. Launch **Wi-Fi Analyzer**.
2. Select **Scan networks**.
3. Follow the scan-status panel.
4. Review nearby networks.
5. Examine per-band congestion.
6. Review channel recommendations.
7. Examine the connected AP.
8. Review the connection-health score.
9. Open **Report** for detailed diagnostics.
10. Choose the appropriate privacy level.
11. Export the report.

---

# 📊 Networks Workspace

The **Networks** workspace provides interactive access to scan results.

It supports:

* Network filtering
* Band filtering
* Security filtering
* Connected-AP identification
* Signal information
* Channel information
* Per-band congestion visualization

![Networks tab showing the scan results table, filters, and per-band congestion charts](images/scan_Main-Networks.png)

---

# 📄 Report Workspace

The Report workspace is organized for IT review and export.

## Overview

Provides a high-level summary of:

* Connection
* Device
* System
* Recommendations
* Scan scope
* Privacy mode
* Connection health
* Channel recommendations
* Recommendation confidence
* Recommendation rationale

![Report Overview tab with connection summary, device details, and per-band channel recommendations](images/scan_Overview.png)

## Networks

Provides the concise day-to-day Wi-Fi list used during troubleshooting.

![Report Networks tab listing nearby networks with band, channel, signal, RSSI, and security](images/scan_Network.png)

## Diagnostics

Provides the detailed radio-observation table, including:

* BSSID
* Frequency
* Channel
* Channel width
* RSSI
* Connected AP
* Additional radio information

![Report Diagnostics tab showing the full radio observation table](images/scan_Diagnostics.png)

---

# 👤 Export Report Information

Before exporting a PDF or CSV, the application can collect optional requestor information.

Available fields include:

* Name
* ID number
* Email
* Building
* Room

Every field is optional.

| Empty                                                                    | Completed                                                                                      |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| ![Export Report Info dialog with empty fields](images/Reporter-Name.png) | ![Export Report Info dialog with requestor details filled in](images/Reporter-Name-filled.png) |

When included, this information appears in the report header.

Requestor information is omitted from redacted reports.

---

# 📑 Professional PDF Reports

The exported PDF can contain:

* Requestor information
* Connection snapshot
* Scan snapshot
* Device information
* System information
* Connection-health score
* Measurements behind each health axis
* Physical-layer information
* Nearby network analysis
* Channel recommendations
* Diagnostic information

![First page of the exported PDF report showing connection health scoring and link detail](images/Report.png)

---

# 🔐 Privacy & Redaction

Wi-Fi Analyzer was designed so diagnostic information can be shared with support teams without unnecessarily exposing identifiers.

## Full Reports

Full reports can contain the device and network identifiers visible to the application.

## Redacted Reports

Redacted exports:

* Replace computer names with aliases
* Replace BSSIDs with per-export salted aliases
* Mask IP addresses
* Mask Wi-Fi interface addresses
* Omit saved contact information
* Can hide SSIDs

Computer model, operating-system version, and architecture remain because they can be necessary for compatibility diagnosis.

## Diagnostic ZIP

Diagnostic ZIP files are **always redacted**.

They also always hide SSIDs.

Diagnostic archives contain only whitelisted:

* Application information
* Settings
* Scan information
* Bounded sanitized logs

See:

`docs/PRIVACY.md`

for the complete field-level privacy policy.

---

# ⌨️ Command-Line Interface

Wi-Fi Analyzer also provides a full command-line interface.

The CLI performs the same multi-pass Wi-Fi scan used by the graphical application and can generate the same report formats.

### Basic Scan

```bash
dotnet run -- --cli
```

### PDF + CSV Export

```bash
dotnet run -- --cli --pdf report.pdf --csv networks.csv --quiet
```

### Redacted Report

```bash
dotnet run -- --cli --redacted --hide-ssids --pdf redacted.pdf
```

### Filtered Scan

```bash
dotnet run -- --cli --match lobby --band 5 --security wpa3 --csv filtered.csv
```

Published executables support the same arguments.

Example:

```bash
WifiAnalyzer --cli --pdf report.pdf
```

---

# ⚙️ CLI Options

| Option                          | Description                                     |
| ------------------------------- | ----------------------------------------------- |
| `--cli`                         | Scan and write the text report to stdout        |
| `--pdf <path>`                  | Generate PDF report                             |
| `--csv <path>`                  | Generate network CSV                            |
| `--quiet`                       | Suppress text output during export              |
| `--redacted`                    | Replace device/network identifiers with aliases |
| `--hide-ssids`                  | Mask SSIDs; requires redacted mode              |
| `--speed-test`                  | Measure throughput, latency, jitter, and loss   |
| `--deep-scan`                   | Enable deep scanning                            |
| `--no-deep-scan`                | Disable deep scanning                           |
| `--name`                        | Requestor name                                  |
| `--id`                          | Requestor ID                                    |
| `--email`                       | Requestor email                                 |
| `--building`                    | Building                                        |
| `--room`                        | Room                                            |
| `--match <text>`                | Filter listed SSID/BSSID values                 |
| `--band <2.4\|5\|6>`            | Filter listed networks by band                  |
| `--security <open\|wpa2\|wpa3>` | Filter listed networks by security              |
| `--help`                        | Display all available options                   |

### Exit Codes

| Code  | Meaning                                             |
| ----- | --------------------------------------------------- |
| `0`   | Success                                             |
| `1`   | Failure or invalid arguments                        |
| `2`   | `--require-network` specified but no networks found |
| `130` | Operation cancelled                                 |

Arguments are validated before accessing the Wi-Fi radio.

---

# 🧑‍💻 Development

## Requirements

Wi-Fi Analyzer is built using:

* .NET 8
* Avalonia
* Native/platform Wi-Fi interfaces
* Platform-specific fallback mechanisms

### macOS Development

```bash
brew install --cask dotnet-sdk
dotnet restore --locked-mode
dotnet run
```

### Privacy-Safe Hardware Test

```bash
dotnet run -- --scanner-smoke --require-network
```

The smoke test reports:

* Scanner type
* Number of networks
* Connection state
* Observed bands
* Computer model
* OS version/build
* Architecture
* Timestamp

It intentionally does **not** output:

* SSIDs
* BSSIDs
* IP addresses
* Wi-Fi interface addresses
* Hostname

---

# 🧪 Testing

Run the automated test suite with:

```bash
dotnet test WifiAnalyzer.Tests/WifiAnalyzer.Tests.csproj \
  -c Release \
  --no-restore
```

Tests cover areas including:

* Scanner fixtures
* Connected AP matching
* Channel evidence
* Export safety
* Privacy/redaction
* Settings
* Diagnostics
* System-information parsing
* Report inclusion
* CLI parsing
* Headless exports

Hosted CI performs locked restore, builds, and tests across:

* Windows
* Linux
* macOS

Real Wi-Fi radio scans are restricted to explicitly approved self-hosted runners.

See:

`docs/TEST_MATRIX.md`

---

# 🪟 Windows Deployment

Wi-Fi Analyzer can be published as a self-contained Windows application.

The target computer does **not** require a separate .NET installation.

### x64 Build

```bash
dotnet restore --locked-mode

dotnet publish WifiAnalyzer.csproj \
  -r win-x64 \
  -c Release \
  --no-restore \
  --output artifacts/publish/win-x64/onefile \
  /p:PublishSingleFile=true \
  /p:SelfContained=true \
  /p:IncludeNativeLibrariesForSelfExtract=true \
  /p:IncludeAllContentForSelfExtract=true
```

### Version 2.5.3 Windows Deliverables

```text
dist/WifiAnalyzer-2.5.3-win-x64-self-contained.exe
dist/WifiAnalyzer-2.5.3-win-x64-self-contained.zip
```

Unsigned Windows builds may trigger Microsoft Defender SmartScreen.

A trusted code-signing certificate is recommended for public distribution.

Additional installer information is available in:

`packaging/windows/README.md`

---

# 🍎 macOS Deployment

The macOS release is distributed as a self-contained `.app` bundle.

The .NET runtime and dependencies are included inside the application, so users do not need to install .NET separately.

### Build

```bash
dotnet publish WifiAnalyzer.csproj \
  -r osx-arm64 \
  -c Release \
  --no-restore \
  --output artifacts/publish/osx-arm64/nosf \
  /p:PublishSingleFile=false \
  /p:SelfContained=true
```

### Package

```bash
packaging/macos/make-app.sh \
  osx-arm64 \
  artifacts/publish/osx-arm64/nosf
```

Without production credentials, the packager applies an ad-hoc signature.

For production distribution, configure:

* `MAC_CODESIGN_IDENTITY`
* `MAC_NOTARY_PROFILE`
* `REQUIRE_SIGNING=true`
* `REQUIRE_NOTARIZATION=true`

The packaging workflow:

1. Signs Mach-O runtime components.
2. Applies hardened-runtime .NET entitlements.
3. Signs the application.
4. Submits the application to Apple.
5. Staples the notarization ticket.
6. Validates the completed bundle.

Output:

```text
dist/Wi-Fi Analyzer.app
dist/WifiAnalyzer-<version>-osx-arm64.zip
```

The most recently documented real-hardware macOS validation is **2.5.3 on Apple silicon running macOS 26.2**: Location Services grant, full SSID and BSSID identifiers, hidden-network handling, scans, and report exports.

---

# 🐧 Linux & Runtime Targets

All supported targets can be generated with:

```bash
./publish-all.sh
```

Published artifacts are stored under:

```text
artifacts/publish/<rid>/{single,nosf}/
```

Supported runtime identifiers:

* `win-x64`
* `win-arm64`
* `osx-x64`
* `osx-arm64`
* `linux-x64`
* `linux-arm64`

---

# 🤖 Release Automation

GitHub Actions automate validation and release workflows.

### CI

`.github/workflows/ci.yml`

Performs deterministic hosted validation.

### Real Device Scanner

`.github/workflows/real-device-scanner.yml`

Runs explicitly authorized Wi-Fi radio tests on protected self-hosted runners.

### macOS Beta

`.github/workflows/macos-beta.yml`

Produces an ARM64 Developer ID-signed and notarized beta from `main` using a protected environment.

### Release

`.github/workflows/release.yml`

Builds supported runtime targets and Windows installers when a version tag exactly matches the version declared in `WifiAnalyzer.csproj`.

For version **2.5.3**:
Open Wifi will lower overall score please base the report on the other numbers.
```text
v2.5.3
```

See:

`CHANGELOG.md`

for the completed 2.5.3 changes.

---

# 🏷️ Product Information

|               |                       |
| ------------- | --------------------- |
| **Product**   | Wi-Fi Analyzer        |
| **Version**   | 2.5.3                 |
| **Publisher** | Ayala Solutions       |
| **Framework** | Avalonia / .NET 8     |
| **Platforms** | Windows, macOS, Linux |
| **Windows**   | x64 / ARM64           |
| **macOS**     | Intel / Apple Silicon |
| **Linux**     | x64 / ARM64           |

### Product Assets

```text
Windows Icon: packaging/windows/icon.ico
macOS Icon:   packaging/macos/AppIcon.icns
```

---

# 📚 Documentation

Additional documentation is available in the repository:

* `docs/USER_GUIDE.md` — Usage, navigation, permissions, reports, and troubleshooting
* `docs/PRIVACY.md` — Field-level privacy and redaction behavior
* `docs/TEST_MATRIX.md` — Testing coverage and platform validation
* `packaging/windows/README.md` — Windows installer and packaging information
* `CHANGELOG.md` — Version history

---

# 🌐 Ayala Solutions Wi-Fi Tools

This project is part of an ongoing effort to provide practical Wi-Fi diagnostic tools for **IT support, Help Desk troubleshooting, network analysis, and end-user diagnostics** across multiple operating systems.

### PowerShell

https://github.com/hov172/PS_WI-FI_Analyzer

### Windows / macOS

https://github.com/hov172/WinWiFiAnalyzer

### Advanced macOS Coverage Mapping

https://github.com/hov172/WifDiagReport

### iOS / iPadOS Beta

https://testflight.apple.com/join/hDPGsHzd

---

# ⭐ Support the Project

If Wi-Fi Analyzer helps with your Wi-Fi troubleshooting or IT support workflow, consider giving the project a ⭐ on GitHub.

Feedback, bug reports, feature requests, testing, and contributions help improve the **Windows, macOS, Linux, PowerShell, and iOS/iPadOS Wi-Fi diagnostic tools**.
