# Sysmon Configuration
Status: Step 8 complete — validated

## Credit

The Sysmon configuration used in this project is not custom-written — it's the community-standard baseline from **[SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)** (source version 74, licensed CC BY 4.0). It was chosen deliberately over writing filtering rules from scratch: it's a well-known, field-tested template that real SOC teams use as a starting baseline, tuned over years to balance detection coverage against noise, rather than reinventing detection logic from zero.

## Goals
- Install Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) on endpoints
- Use a tuned config to capture high-value events
- Validate local Sysmon event generation on `TEST-WIN10-LAN1` before configuring WEF

## Step 8 Scope

Step 8 is local validation only. The first proof point is the `Microsoft-Windows-Sysmon/Operational` log on `TEST-WIN10-LAN1`. WEF forwarding, collector-side validation, and downstream ingestion belong to later steps.

## Implementation Notes

- Sysmon version used: `Sysmon64.exe` v15.21 (Sysinternals)
- Config source: SwiftOnSecurity (`https://github.com/SwiftOnSecurity/sysmon-config`), source version 74, schema version 4.91
- Config file name and location: `sysmonconfig-export.xml` in `C:\Users\Administrator\Downloads\Sysmon\` on `TEST-WIN10-LAN1`
- Install command: `Sysmon64.exe -accepteula -i sysmonconfig-export.xml`
- Key Event IDs observed: 1 (Process Create), 13 (Registry value set), 22 (DNS query)
- Test activity used to generate local events: opening Notepad and an elevated Command Prompt

## Local Validation Checks

- [x] Confirm Sysmon service is running — `Sysmon64` and `SysmonDrv` both started per install console output
- [x] Confirm `Microsoft-Windows-Sysmon/Operational` exists in Event Viewer
- [x] Generate controlled local test activity
- [x] Confirm recent Sysmon events are visible locally — 24 events observed on host `DESKTOP-8K5AHHR.corp.local`
- [x] Record at least one Event ID and what it proves — Event ID 1 (Process Create) for `notepad.exe`, confirming Sysmon captures process-level detail (PID, image path, file version) beyond what the standard Windows Security log provides

## Evidence

- `screenshots/step08-sysmon64-install-success.png` — install console output
- `screenshots/step08-local-activity-generated.png` — Notepad + Command Prompt opened to generate activity
- `screenshots/step08-sysmon-operational-log-events-confirmed.png` — Sysmon Operational log, 24 events confirmed
- Full click-by-click steps and troubleshooting notes: private runbook, `step08-sysmon-local-validation.md`
