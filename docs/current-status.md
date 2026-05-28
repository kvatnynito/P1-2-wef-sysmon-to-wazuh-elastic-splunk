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

Splunk was installed and its Web UI was validated during P1-1. In P1-2, pfSense syslog ingestion has now been validated, and Windows Event Log ingestion is still planned.

## Current Milestone

**Active milestone:** Milestone 6 — Logging Foundation

## Completed P1-2 Milestones

None yet.

## Current Objective

The current objective is to prove initial log flow into Splunk before expanding the telemetry design or adding more hosts.

Milestone 6 focuses on validating two initial log sources:

- pfSense logs forwarded to `SIEM-SPLUNK01`
- Windows Event Logs forwarded from `TEST-WIN10-LAN1` to `SIEM-SPLUNK01`

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
| `TEST-WIN10-LAN1` | `SIEM-SPLUNK01` | Splunk Universal Forwarder | TCP 9997 | Planned |

## Current Progress

- P1-1 segmented lab foundation is complete.
- Splunk is installed on `SIEM-SPLUNK01`.
- Splunk Web UI was validated during P1-1.
- Splunk Web UI was revalidated from `TEST-WIN10-LAN1` at `http://10.10.10.20:8000`.
- Splunk UDP network input for pfSense syslog was configured on UDP `5514` with sourcetype `syslog` and index `default` / `main`.
- pfSense `Status > System Logs > Settings` page was reached for remote logging configuration.
- pfSense remote logging configured to forward to SIEM-SPLUNK01 on UDP 5514.
- pfSense logs validated in Splunk — 901 events returned, host=10.10.10.1, source=udp:5514, sourcetype=syslog, filterlog entries confirmed.
- P1-2 telemetry ingestion validation is in progress.
- Windows Event Log forwarding has not yet been configured.
- Windows logs have not yet been validated in Splunk.
- Sysmon has not been deployed.
- WEF has not been configured.
- Wazuh ingestion has not been validated.
- Elastic ingestion has not been validated.

## Current Rule

Do not add more VMs during Milestone 6 unless Splunk logging is working first.

Milestone 6 must prove that Splunk can receive and display logs from the initial pfSense and Windows sources before the project moves into collector placement, Sysmon, WEF, Wazuh, Elastic, or additional telemetry-supporting hosts.

## Next Actions

1. Configure Windows Event Log forwarding from `TEST-WIN10-LAN1`.
2. Validate Windows logs in Splunk.
3. Document source IPs, ports, screenshots, and validation searches.

## Evidence Captured

- `screenshots/milestone06-siem-splunk01-ip-confirmed.png`
- `screenshots/milestone06-splunk-service-running.png`
- `screenshots/milestone06-splunk-web-ui-reachable-from-win10.png`
- `screenshots/milestone06-splunk-udp5514-input-configured.png`
- `screenshots/milestone06-pfsense-remote-logging-configured-udp5514.png`
- `screenshots/milestone06-splunk-pfsense-events-visible.png` — pfSense validation, 901 events

## Milestone 6 Completion Criteria

Milestone 6 is complete only when:

- pfSense forwards logs to `SIEM-SPLUNK01`.
- Splunk displays pfSense logs.
- `TEST-WIN10-LAN1` sends logs to Splunk.
- Splunk displays Windows logs.
- Documentation includes source IPs, ports, screenshots, and validation searches.

## Current Status Summary

P1-2 is in Milestone 6.

The lab foundation exists from P1-1, and Splunk is available as the initial telemetry destination. Splunk is configured to listen for pfSense syslog on UDP `5514`. pfSense remote logging is now active and validated — 901 events were confirmed in Splunk with host=10.10.10.1, source=udp:5514, sourcetype=syslog, and filterlog entries visible. The next step is Windows Event Log forwarding from `TEST-WIN10-LAN1`.
