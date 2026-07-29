# Report and diagnostic privacy

Wi‑Fi scans can contain location-sensitive network identifiers. Wi‑Fi Analyzer
keeps export behavior explicit and applies redaction at the report boundary.

## Full report

A Full report can contain:

- Computer name, model, OS version/build, and architecture
- IP and Wi-Fi interface addresses
- Connected SSID and BSSID
- Nearby SSIDs and BSSIDs
- Channel, frequency, width, signal, RSSI, and security measurements
- PHY link rates, standard, modulation detail, noise floor, and SNR
- Channel utilization and associated-station counts advertised by access points
- Roaming candidates for the connected SSID, listed by BSSID
- Speed-test results, including the endpoint host used for loss and jitter probes
- User/contact fields entered for the report

Use Full only when the recipient is authorized to receive those identifiers.

The roaming section lists one BSSID per candidate access point. Those are the
same identifiers already present in the nearby-network list and are redacted by
the same rule, but the section groups them by SSID, which makes the physical
layout of a site easier to infer. Treat a Full roaming report as site
information, not only as device information.

## Redacted report

Redacted PDF, CSV, and text reports:

- Replace the computer name with a salted per-export device alias.
- Mask the IP address.
- Mask the Wi-Fi interface address.
- Replace BSSIDs with salted per-export aliases.
- Remove saved user/contact information.
- Preserve SSIDs by default, or replace them with `[hidden]` when
  **Hide SSIDs when redacted** is enabled.
- Preserve computer model, OS version/build, and architecture because they are
  support compatibility facts rather than direct account/contact identifiers.
- Preserve radio measurements — link rates, modulation, utilization, loss, and
  jitter — because they describe the link's behavior rather than identifying a
  device or a person.

Redaction is applied before the roaming, DFS, and health sections are built, so
those sections reference the same salted aliases as the network list rather than
real BSSIDs.

The random salt is created for each redaction context. Aliases remain
consistent within one generated report so the connected access point can still
be matched to its row, but they are not intended for correlation across
separate exports.

## Diagnostic ZIP

Diagnostic ZIP exports always use Redacted mode with SSIDs hidden. They contain
only whitelisted files:

- Manifest and application metadata
- Sanitized settings metadata
- Redacted scan data and recommendation evidence
- Bounded, sanitized recent logs

The bundle excludes saved contact information, configured speed-test URLs,
tokens, authorization headers, emails, raw IP/MAC/BSSID values, and SSIDs.

## Local settings and logs

Settings are stored in the current user’s local application-data directory.
On Unix-like systems the settings file is restricted to the current user.
Remembering report contact information is opt-in; disabling it removes saved
contact data from settings.

Logs are rotated and bounded. Diagnostic export sanitizes them again before
including them in a ZIP.

## Scanner-smoke output

`--scanner-smoke` is designed for CI and hardware validation. It emits:

- Scanner implementation
- Network count and connected/not-connected state
- Observed bands
- Computer model, OS version/build, and architecture
- Scan timestamp

It does not emit SSIDs, BSSIDs, hostname, IP address, Wi-Fi interface address,
or contact information.
