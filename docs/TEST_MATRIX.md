# Validation and release test matrix

Hosted validation deliberately separates deterministic parser/build checks from
hardware-dependent Wi-Fi scans. GitHub-hosted runners generally do not expose a
usable wireless adapter or the OS permissions needed for a real scan.

| Validation | Windows | Linux | macOS | Trigger |
|---|---:|---:|---:|---|
| Locked restore and Release build | Yes | Yes | Yes | Pull request and `main` push |
| Sanitized scanner-fixture parsers | Yes | Yes | Yes | Pull request and `main` push |
| Channel/export/settings/system-info unit tests | Yes | Yes | Yes | Pull request and `main` push |
| Real Wi-Fi scan | Self-hosted | Self-hosted | Self-hosted | Manual, acknowledged |
| Portable release package | x64 + ARM64 | x64 + ARM64 | x64 + ARM64 | Version tag |
| Signed/notarized beta | — | — | ARM64 | Manual, protected environment |

## Scanner fixtures

Representative, sanitized command output lives under
`WifiAnalyzer.Tests/Fixtures/`:

- `windows/netsh-interfaces.txt` and `windows/netsh-networks-bssid.txt`
- `linux/nmcli-wifi-list.txt`
- `macos/system-profiler.json`

The fixture tests run on every hosted operating system. They protect parsing
behavior without depending on the host language, radio hardware, nearby
networks, or location permissions. Fixtures must never contain real employee,
customer, home, or campus SSIDs/BSSIDs.

## Real-device validation

Run the `Real-device Wi-Fi scanner validation` workflow manually. It requires:

1. An explicit acknowledgement that the runner is approved to inspect its
   nearby wireless environment.
2. A protected `wifi-scanner-validation` GitHub Environment.
3. A self-hosted runner carrying `wifi-scanner` and exactly one platform label:
   `windows`, `linux`, or `macos`.
4. .NET 8 and the platform scanner prerequisite:
   - Windows: WLAN AutoConfig and an enabled Wi-Fi adapter.
   - Linux: NetworkManager/`nmcli` and permission to query Wi-Fi.
   - macOS: Wi-Fi enabled and Location Services permission granted to the
     self-hosted runner process.

The workflow runs `--scanner-smoke --require-network`. The command fails unless
the platform scanner detects at least one network. Its JSON contains scanner
type, network count, connected/not-connected state, observed bands, computer
model, OS version/build, architecture, and timestamp. It does not emit SSIDs,
BSSIDs, hostname, IP address, interface address, contact information, or other
nearby-network identifiers, and the workflow does not redirect output to disk.
Use an ephemeral or access-controlled runner because self-hosted machines
retain whatever their operator independently configures outside the workflow.

## Current regression coverage

The 2.4.0 baseline contains 179 passing tests. In addition to the platform
fixtures, the suite validates:

- Connected-access-point matching, ordering, and unavailable-identifier behavior
- Channel normalization, overlap scores, confidence, and recommendation evidence
- PDF pagination, CSV escaping/formula neutralization, and output-path safety
- Full/redacted boundaries and always-redacted diagnostic ZIP contents
- Settings migration/default behavior and injected application services
- macOS model and `sw_vers` parsing plus inclusion of system data in reports
- MAC/BSSID hexadecimal formatting, including non-MAC values such as redaction
  labels and masked forms, which must pass through untouched
- Beacon information-element decoding: BSS Load utilization and station counts,
  Wi-Fi 4/5/6/6E/7 identification, spatial streams, 802.11k/v/r advertisement,
  country code, and truncated elements that must not read out of bounds
- Generation-to-amendment naming, including the band-specific legacy cases
  (802.11a on 5 GHz, 802.11b/g on 2.4 GHz) and the 6 GHz qualifier that
  separates Wi-Fi 6E from Wi-Fi 6 where both report 802.11ax
- PHY rate back-solving against the 802.11n/ac/ax/be rate tables, including the
  cases where no rate was reported and where a rate matches no table entry and
  must be declined rather than guessed
- Loss and jitter statistics, including mean consecutive deviation and the
  single-probe case where jitter is not computable
- Health scoring per axis, exclusion of unmeasured axes from the weighting, the
  security ranking order, and preference for measured airtime over AP counts
- Roaming candidate selection, signal deltas, and same-radio identification
- DFS classification, congestion scoring for channels the recommendation
  candidates omit, and exclusion of an access point's own signal from its
  channel score
- Empty-scan anomaly detection, and that location is named as a cause only when
  a Windows setting positively denies it
- Linux `iw` link parsing for rates, MCS, streams, width, and guard interval
- Command-line parsing, including options rejected before the radio is touched
  and options that warn rather than take effect silently
- Headless export parity: the merged multi-pass scan, the single-pass override,
  connected-SSID resolution, requestor detail in each privacy mode, and a speed
  test that fails without losing the report

## Manual interface checks

Perform these checks on a real desktop before promoting a release:

| Check | Expected result |
|---|---|
| Fresh launch | Only the main Wi‑Fi Analyzer window opens |
| macOS application menu | `About Wi‑Fi Analyzer…`, `Hide Wi‑Fi Analyzer`, and product-branded Quit item |
| About action | Branded, wrapped About dialog; no Avalonia product dialog |
| Scan on a connected adapter | Networks list is populated; no incomplete-scan banner appears |
| Incomplete-scan banner | Appears only when a connected adapter returned no networks; the location-settings button appears only when a Windows setting denies location |
| Health score with no speed test | Headline is labelled provisional and names the measured weight |
| Link detail on Windows | Modulation is marked `[derived]`; noise floor states that Windows does not expose it |
| Scan defaults | Deep scan enabled and Skip speed test unchecked |
| Scan status | Progress panel remains above the workspace and does not overlay lists |
| Report navigation | Overview, Networks, and Diagnostics fit within the report workspace |
| Connected AP | Matching BSSID row is first and marked `● Connected` |
| New scan system data | Model, friendly OS version/build, and architecture appear in Overview and Diagnostics |
| Redacted export | Identifiers are replaced/masked and contact data is absent |
| Diagnostic ZIP | SSIDs are hidden and only whitelisted files are present |

## Release-artifact validation

For macOS, validation is performed against the app extracted from the final ZIP,
not only the source bundle:

1. `codesign --verify --deep --strict`
2. `xcrun stapler validate`
3. `spctl --assess --type execute`
4. Packaged `--scanner-smoke`

The production app is a self-contained multi-file `.app` payload. Every Mach-O
runtime library/helper is signed before the main executable and outer bundle.
The final app name is architecture-neutral; architecture remains in the archive
filename.

For the Windows portable ZIP, verify that it contains exactly one versioned
PE32+ x64 GUI executable at the archive root. The one-file publish uses
`IncludeNativeLibrariesForSelfExtract` and `IncludeAllContentForSelfExtract`.

## Signed and notarized macOS beta

The manual `Signed and notarized macOS beta` workflow runs only from `main` and
uses the protected `macos-beta-signing` environment. Configure required
reviewers for that environment and all of these environment secrets:

- `MACOS_CERTIFICATE_P12`: base64-encoded Developer ID Application `.p12`
- `MACOS_CERTIFICATE_PASSWORD`: password for the `.p12`
- `MACOS_SIGNING_IDENTITY`: exact Developer ID Application identity name
- `APPLE_NOTARY_KEY_P8`: base64-encoded App Store Connect API private key
- `APPLE_NOTARY_KEY_ID`: App Store Connect key ID
- `APPLE_NOTARY_ISSUER_ID`: App Store Connect issuer ID

The `main` restriction prevents a caller from selecting an arbitrary checkout
to run with signing credentials. The job rejects incomplete credentials before
publishing. It imports the
certificate into a temporary keychain, signs with hardened runtime and a secure
timestamp, submits with `notarytool`, staples and validates the ticket, assesses
the app with Gatekeeper, and uploads only the final archive. A trap deletes the
temporary keychain, certificate, and API key on success or failure.

The beta workflow creates an artifact only; it does not create or modify a
GitHub Release. Production version-tag releases remain controlled by
`.github/workflows/release.yml`.
