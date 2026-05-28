# Current Status

## P1-1 Completed Tasks

- SIEM-SPLUNK01 VM was created.
- Ubuntu Server was installed on SIEM-SPLUNK01.
- System update/upgrade was completed.
- SIEM-SPLUNK01 was confirmed on static IP `10.10.10.20`.
- LAN1 gateway, outbound IP reachability, and DNS resolution were validated.
- Splunk Enterprise 10.2.3 was installed from a Linux `.deb` package.
- Splunk was configured to run as the `splunk` service user.
- Splunk boot-start was enabled through `systemd`.
- Splunk Web UI was validated from TEST-WIN10-LAN1 at `http://10.10.10.20:8000`.

## Project State

P1-2 is now active.

This project begins after completion of `P1-1-proxmox-segmentation-lab`, which established the segmented Proxmox lab foundation.

P1-2 focuses on telemetry collection, forwarding, validation, and investigation readiness.

Splunk was installed and its Web UI was validated during P1-1. In P1-2, pfSense syslog ingestion and Windows Event Log ingestion have both been validated.

## Current Milestone

**Active milestone:** Milestone 7 — Collector Placement and First Endpoint Prep

## Completed P1-2 Milestones

- Milestone 6 — Logging Foundation

## Current Objective

Milestone 6 is complete. The current objective is to decide collector placement and prepare the first Windows endpoint for the next telemetry phase.

Milestone 7 focuses on:

- Deciding whether the WEF collector will be `AD-DC01` or a dedicated `WEC01`
- Documenting the reasoning and design tradeoffs
- Confirming the first endpoint is ready for WEF and Sysmon onboarding

## Current Lab Systems Available from P1-1

| System | Role | Status |
|---|---|---|
| `FW-EDGE01` | pfSense firewall/router | Available from P1-1 |
| `AD-DC01` | Domain Controller / possible collector candidate | Available from P1-1 |
| `TEST-WIN10-LAN1` | First Windows endpoint / future AD-WIN10 | Available from P1-1 |
| `TEST-WIN10-LAN2` | LAN2 Windows endpoint | Available from P1-1 |
| `ATTACK-KALI01` | Kali traffic generator | Available from P1-1 |
| `VULN-METASPLOITABLE2` | Vulnerable Linux target | Available from P1-1 |
| `SIEM-SPLUNK01` | Splunk destination | Available from P1-1 |

## Milestone 6 Log Source Map

| Source | Destination | Method | Port | Current Status |
|---|---|---|---:|---|
| `FW-EDGE01` / pfSense | `SIEM-SPLUNK01` | Syslog | UDP 5514 | Validated - 901 events confirmed in Splunk, host=10.10.10.1, udp:5514 |
| `TEST-WIN10-LAN1` | `SIEM-SPLUNK01` | Splunk Universal Forwarder | TCP 9997 | Validated - WinEventLog:Security events confirmed in Splunk, host=DESKTOP-8K5AHHR |

## Current Progress

- P1-1 segmented lab foundation is complete.
- Splunk is installed on `SIEM-SPLUNK01`.
- Splunk Web UI was validated during P1-1.
- Splunk Web UI was revalidated from `TEST-WIN10-LAN1` at `http://10.10.10.20:8000`.
- Splunk UDP network input for pfSense syslog was configured on UDP `5514` with sourcetype `syslog` and index `default` / `main`.
- pfSense `Status > System Logs > Settings` page was reached for remote logging configuration.
- pfSense remote logging configured to forward to SIEM-SPLUNK01 on UDP 5514.
- pfSense logs validated in Splunk — 901 events returned, host=10.10.10.1, source=udp:5514, sourcetype=syslog, filterlog entries confirmed.
- Milestone 6 complete — both initial log sources validated in Splunk.
- Splunk Universal Forwarder installed on `TEST-WIN10-LAN1`, pointed at `10.10.10.20:9997`.
- `inputs.conf` manually created with Application, Security, and System channels enabled.
- Windows Event Log forwarding validated — WinEventLog:Security events confirmed in Splunk, host=DESKTOP-8K5AHHR, 17+ events visible.
- Sysmon has not been deployed.
- WEF has not been configured.
- Wazuh ingestion has not been validated.
- Elastic ingestion has not been validated.

## Current Rule

Do not begin Sysmon deployment or WEF configuration until the collector placement decision is documented and the first endpoint is confirmed ready.

## Next Actions

1. Decide collector placement — `AD-DC01` or dedicated `WEC01`. Document the reasoning.
2. Confirm `TEST-WIN10-LAN1` is ready for WEF and Sysmon onboarding.
3. Document the decision and endpoint readiness in GitHub.

## Evidence Captured

- `screenshots/milestone06-siem-splunk01-ip-confirmed.png`
- `screenshots/milestone06-splunk-service-running.png`
- `screenshots/milestone06-splunk-web-ui-reachable-from-win10.png`
- `screenshots/milestone06-splunk-udp5514-input-configured.png`
- `screenshots/milestone06-pfsense-remote-logging-configured-udp5514.png`
- `screenshots/milestone06-splunk-pfsense-events-visible.png` — pfSense validation, 901 events
- `screenshots/milestone06-splunk-tcp9997-receiving-enabled.png` — Splunk TCP 9997 receiving port configured and enabled
- `screenshots/milestone06-splunk-windows-events-visible.png` — Windows Event Log validation, WinEventLog:Security events confirmed from DESKTOP-8K5AHHR

## Milestone 6 Completion Criteria

Milestone 6 is complete only when:

- pfSense forwards logs to `SIEM-SPLUNK01`.
- Splunk displays pfSense logs.
- `TEST-WIN10-LAN1` sends logs to Splunk.
- Splunk displays Windows logs.
- Documentation includes source IPs, ports, screenshots, and validation searches.

## Current Status Summary

P1-2 is in Milestone 7 — Collector Placement and First Endpoint Prep.

Milestone 6 is complete. pfSense syslog (901+ events, host=10.10.10.1, UDP 5514) and Windows Event Log forwarding (WinEventLog:Security, host=DESKTOP-8K5AHHR, TCP 9997) are both validated in Splunk. The next step is deciding whether the WEF collector will be `AD-DC01` or a dedicated `WEC01`, and confirming the first endpoint is ready for WEF and Sysmon onboarding.
