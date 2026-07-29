# Wi‑Fi Analyzer user guide

## Start a scan

The first-run defaults are:

- **Deep scan:** enabled
- **Speed test:** enabled (`Skip speed test` is unchecked)
- **Report detail:** Full

Select **Scan networks**. The application displays progress in a dedicated
status panel below the scan options. **Cancel** stops the active operation.

Deep scan requests the fullest network identifiers the operating system allows.
On macOS, allow Wi‑Fi Analyzer under **System Settings → Privacy & Security →
Location Services**. Without that permission, macOS can withhold SSID and BSSID
values even while channel and signal measurements remain available.

## Main Networks workspace

The top-level **Networks** tab is the interactive analysis workspace. It
contains:

- Search by SSID or BSSID
- Band and security filters
- A sortable network grid
- Channel-congestion charts

The connected network summary above the workspace shows SSID, BSSID,
channel/width, RSSI/SNR when available, and recommended channels.

## Report workspace

The top-level **Report** tab has three secondary views so the complete report
does not compete for vertical space.

### Overview

Use Overview for the support summary:

- Connected network and radio measurements
- Number of observed networks and bands
- Report privacy mode and capture time
- Computer name, model, OS version/build, architecture, IP, and Wi-Fi address
- Recommended 2.4, 5, and observed 6 GHz channels
- Recommendation confidence and rationale
- Connection health score with its per-axis breakdown

System information is captured at scan time. Run a new scan after installing an
updated build to populate newly added fields.

### Reading the connection health score

The score runs from 0 to 100 across five weighted axes: Signal (25%),
Speed (25%), Congestion (20%), Security (15%), and Reliability (15%). Each axis
states the measurement behind it, so a rating can always be traced to a number.

An axis that could not be measured is shown as unmeasured and left out of the
weighting rather than treated as good. When less than half the weighting could
be measured the score is labelled **provisional** — for example, a scan with no
speed test and no association is scored on signal alone, and a headline of
100/100 there means only that the signal is strong.

### Reading link detail

The report separates two numbers that are easy to confuse:

- **PHY link rate** is the speed the radio negotiated over the air.
- **Download/upload** is the throughput actually achieved.

Real throughput is typically 40-60% of the PHY rate. Throughput far below that
points to the path beyond the access point rather than to the radio link, and
the report says so when it sees that pattern.

The standard is given as both the marketing generation and the IEEE amendment —
"Wi-Fi 5" with "802.11ac" beside it — because equipment documentation and
support articles use whichever of the two suits them. Wi-Fi 6 and Wi-Fi 6E are
both 802.11ax, so the 6 GHz band is named to separate them. An access point too
old to advertise an HT element is shown as legacy against the band it is on:
802.11a on 5 GHz, 802.11b/g on 2.4 GHz.

The generation is read from what the access point advertises in its beacon, not
from what the current link negotiated. An access point capable of Wi-Fi 6 will
read as Wi-Fi 6 even when this client associates at a lower rate.

Modulation detail is marked either as reported by the operating system or as
derived. Windows and macOS do not expose the negotiated MCS index, spatial
stream count, or guard interval through any public API, so on those platforms
the values are back-solved from the link rate against the 802.11 rate tables and
labelled `[derived]`. Linux reports them directly through `iw`. Where more than
one modulation combination produces the same rate, the report says the result is
indicative.

### Reading channel congestion

Two different measures appear, and they answer different questions:

- **Overlap score** is computed from the observed access points and their signal
  strength. It is always available.
- **Channel utilization** is the percentage of airtime the medium was actually
  busy, taken from an access point's BSS Load element. It is the better measure
  because it reflects how much neighbours transmit, not merely how many exist —
  but not every access point advertises it.

When no access point in range advertises a BSS Load element, the report says so
rather than leaving the absence unexplained.

### Reading roaming and DFS

The roaming section lists every access point serving the connected SSID, with
each one's signal difference against the current association. The roam trigger
and margin shown are industry guidance, not values read from the adapter: the
real thresholds live in vendor-specific driver settings that no public operating
system API exposes.

DFS channels are shared with radar. An access point on one must vacate it within
seconds of detecting a radar pulse, which briefly drops every client, and some
clients discover DFS channels more slowly. The report explains whether the
measured congestion justifies that trade, comparing the current channel against
the best non-DFS alternative. It cannot read the access point's channel
selection logic, so this is inference from the local scan.

### Networks

Networks is the concise everyday list. It shows connected status, network name,
band, channel, signal percentage, RSSI, and security.

When the connected BSSID is available, its row is placed first and marked
**● Connected**. If the operating system withholds the BSSID, the app does not
guess; it displays identifier-availability guidance instead.

### Diagnostics

Diagnostics is the structured technical list for IT and troubleshooting. It
adds BSSID, frequency, channel width, and the full platform measurements to the
concise network information.

Networks and Diagnostics intentionally share the same scan data:

- Use **Networks** for quick signal/security review.
- Use **Diagnostics** when exact radio identifiers and measurements are needed.

## Export reports

The speed test also measures packet loss and jitter using a burst of probes sent
before the transfers, so the figures describe an otherwise idle link rather than
congestion the test created itself. Latency alone hides the two faults that
actually break calls and remote desktop sessions. Where ICMP is filtered the app
falls back to TCP handshake probes and says so, noting that the kernel
retransmits a lost SYN, so isolated loss then appears as a latency spike and the
loss figure should be read as a floor.

The report toolbar offers:

- **Save PDF:** formatted multi-page support report
- **Save CSV:** network measurement table for spreadsheet analysis
- **Export detail:** Full or Redacted
- **Hide SSIDs when redacted:** available only in Redacted mode

PDF and the on-screen Overview include device/system context. CSV remains a
network-focused table and does not add metadata rows that would disrupt
spreadsheet imports.

## Diagnostics and support

- **Copy diagnostics** copies a plain-text support payload.
- **Diagnostic ZIP** writes an always-redacted bundle with hidden SSIDs.
- **Help → About Wi‑Fi Analyzer** opens product/version information.
- On macOS, **Wi‑Fi Analyzer → About Wi‑Fi Analyzer…** opens the same branded
  dialog.

The About window opens only when selected. It does not appear at application
startup.

## Platform notes

### Windows

Windows 10 and Windows 11 Home, Pro, and managed editions use the same
application workflow. Native controls and typography follow Windows. WLAN
AutoConfig must be running.

Windows does not expose a noise-floor measurement to applications, so SNR cannot
be calculated there. The report shows RSSI and states this rather than leaving
the field blank.

Since Windows 10 version 2004, Windows treats BSSID, channel, and band as
location data. Without location access the scan APIs still succeed but return no
access points, which would otherwise look like an empty airwave. If that
happens, the Networks tab explains it and offers a button that opens
**Settings → Privacy & security → Location**; enable **Let desktop apps access
your location**, then scan again. The application cannot grant itself this
permission, so only that setting changes it.

If a scan reports no access points while the adapter is connected and location
access is not being denied, the scan itself returned nothing. Run it again; if
it persists, restart the WLAN AutoConfig service.

The portable x64 EXE is self-contained and requires no separate .NET runtime.
An unsigned build may show a SmartScreen warning.

### macOS

The Dock, application menu, Quit item, window title, and About item use
**Wi‑Fi Analyzer**. Nearby network names require Location Services permission.
The signed release ZIP contains a self-contained, notarized app bundle.

macOS is the only desktop platform that publishes a noise-floor measurement, so
SNR is a real reading there rather than an estimate.

### Linux

NetworkManager and `nmcli` are required. Desktop integration varies by
distribution, but scan/report behavior is the same.

Install `iw` for full link detail. Linux is the only platform that reports the
MCS index, spatial-stream count, channel width, and guard interval directly, so
those values are not derived there. The noise floor is read from
`/proc/net/wireless` when the driver publishes it.
