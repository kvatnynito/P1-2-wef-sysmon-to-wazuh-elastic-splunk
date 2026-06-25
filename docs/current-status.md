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

This project begins after completion of `P1-1-proxmox-segmentation-lab`, which established the segmented Proxmox (an open-source virtualization platform used to run multiple virtual machines on one physical host) lab foundation.

P1-2 focuses on telemetry (the stream of log and event data collected from systems to support monitoring and investigation) collection, forwarding, validation, and investigation readiness.

Splunk was installed and its Web UI was validated during P1-1. In P1-2, pfSense (an open-source firewall and router acting as the network gateway in this lab) syslog (a standard format that network devices like firewalls use to send log messages) ingestion (the process of receiving log data into a platform like Splunk) and Windows Event Log ingestion have both been validated.

## Current Milestone

**Active milestone:** Milestone 8 — Sysmon Deployment and Local Validation

## Completed P1-2 Milestones

- Milestone 6 — Logging Foundation
- Milestone 7 — Collector Placement and First Endpoint Prep

## Current Objective

Milestones 6 and 7 are complete. Collector placement has been decided: a dedicated `WEC01` collector will be used. `TEST-WIN10-LAN1` has been joined to `corp.local` and confirmed ready for WEF and Sysmon onboarding. The current objective is to deploy Sysmon on the first Windows endpoint and validate local Sysmon event generation before configuring WEF.

Milestone 8 focuses on:

- Deploying Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) to `TEST-WIN10-LAN1`
- Applying a controlled Sysmon configuration
- Generating basic local endpoint activity
- Confirming Sysmon events are visible locally before WEF forwarding is configured

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
| `FW-EDGE01` / pfSense | `SIEM-SPLUNK01` (SIEM — Security Information and Event Management — a platform that collects, stores, and searches log data from many sources) | Syslog | UDP (User Datagram Protocol — a fast, connectionless way to send data over a network, common for log delivery) 5514 | Validated - 901 events confirmed in Splunk, host=10.10.10.1, udp:5514 |
| `TEST-WIN10-LAN1` | `SIEM-SPLUNK01` | Splunk Universal Forwarder (a lightweight agent installed on an endpoint that ships its logs to a central SIEM) | TCP (Transmission Control Protocol — a reliable, connection-based way to send data that confirms delivery) 9997 | Validated - WinEventLog:Security events confirmed in Splunk, host=DESKTOP-8K5AHHR |

## Current Progress

- P1-1 segmented lab foundation is complete.
- Splunk is installed on `SIEM-SPLUNK01`.
- Splunk Web UI was validated during P1-1.
- Splunk Web UI was revalidated from `TEST-WIN10-LAN1` at `http://10.10.10.20:8000`.
- Splunk UDP network input for pfSense syslog was configured on UDP `5514` with sourcetype (a label Splunk uses to identify what kind of log data came in, so it knows how to parse it) `syslog` and index (the storage bucket Splunk uses to organize incoming log data) `default` / `main`.
- pfSense `Status > System Logs > Settings` page was reached for remote logging configuration.
- pfSense remote logging configured to forward to SIEM-SPLUNK01 on UDP 5514.
- pfSense logs validated in Splunk — 901 events returned, host=10.10.10.1, source=udp:5514, sourcetype=syslog, filterlog (pfSense's built-in logging format for firewall rule activity) entries confirmed.
- Milestone 6 complete — both initial log sources validated in Splunk.
- Splunk Universal Forwarder installed on `TEST-WIN10-LAN1`, pointed at `10.10.10.20:9997`.
- `inputs.conf` (a Splunk forwarder configuration file that defines which logs to collect and send) manually created with Application, Security, and System channels enabled.
- Windows Event Log forwarding validated — WinEventLog:Security events confirmed in Splunk, host=DESKTOP-8K5AHHR, 17+ events visible.
- Collector placement decision made: dedicated `WEC01` chosen over `AD-DC01`; role separation is production standard and matches a real SOC environment.
- `WEC01` VM provisioned in Proxmox (VMID 107, Windows Server 2022, LAN1, static IP 10.10.10.30).
- `WEC01` computer name set, static IP assigned (10.10.10.30), DNS pointed at AD-DC01 (10.10.10.10).
- `AD-DC01` promoted to domain controller — `corp.local` domain created (forest and domain functional level: Windows Server 2016, DNS on AD-DC01).
- `WEC01` successfully joined to the `corp.local` domain.
- `WEC01` domain membership confirmed in Server Manager: computer name `WEC01`, domain `corp.local`, Ethernet `10.10.10.30`.
- `WEC01` domain controller discovery validated with `nltest /dsgetdc:corp.local`; `AD-DC01.corp.local` resolved at `10.10.10.10`.
- `WEC01` network configuration validated: static IPv4 `10.10.10.30`, gateway `10.10.10.1`, DNS server `10.10.10.10`, primary DNS suffix `corp.local`.
- pfSense DHCP for LAN1 updated to hand out `AD-DC01` (`10.10.10.10`) as DNS while keeping pfSense (`10.10.10.1`) as DHCP server and default gateway.
- `TEST-WIN10-LAN1` joined to the `corp.local` domain and validated with `whoami` returning `corp\administrator`.
- `TEST-WIN10-LAN1` DNS readiness validated: primary DNS suffix `corp.local`, DNS server `10.10.10.10`, and `nslookup corp.local` resolving through `AD-DC01`.
- `TEST-WIN10-LAN1` collector reachability validated by resolving `WEC01.corp.local` to `10.10.10.30` and confirming WinRM TCP `5985` to `WEC01` succeeds. ICMP ping to `WEC01` is blocked or unvalidated, but the WEF-relevant WinRM path is reachable.
- Splunk continuity checked after the domain join: `Splunkd.service` is active on `SIEM-SPLUNK01`, and `SplunkForwarder` remains Running / Automatic on `TEST-WIN10-LAN1`.
- Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) has not been deployed.
- WEF has not been configured.
- Wazuh (an open-source security platform that collects agent data, generates alerts, and supports endpoint monitoring) ingestion has not been validated.
- Elastic (the Elasticsearch and Kibana stack — a search and visualization platform used to store and query log data) ingestion has not been validated.

## Current Rule

Do not begin WEF configuration until Sysmon has been deployed and validated locally on the first endpoint.

## Next Actions

1. Download or stage Sysmon and the selected Sysmon configuration on `TEST-WIN10-LAN1`.
2. Install Sysmon on `TEST-WIN10-LAN1`.
3. Generate basic local endpoint activity.
4. Confirm Sysmon service state and local Sysmon events in Event Viewer.
5. Document Milestone 8 validation before starting WEF subscriptions.

## Evidence Captured

- `screenshots/milestone06-siem-splunk01-ip-confirmed.png`
- `screenshots/milestone06-splunk-service-running.png`
- `screenshots/milestone06-splunk-web-ui-reachable-from-win10.png`
- `screenshots/milestone06-splunk-udp5514-input-configured.png`
- `screenshots/milestone06-pfsense-remote-logging-configured-udp5514.png`
- `screenshots/milestone06-splunk-pfsense-events-visible.png` — pfSense validation, 901 events
- `screenshots/milestone06-splunk-tcp9997-receiving-enabled.png` — Splunk TCP 9997 receiving port configured and enabled
- `screenshots/milestone06-splunk-windows-events-visible.png` — Windows Event Log validation, WinEventLog:Security events confirmed from DESKTOP-8K5AHHR
- `screenshots/milestone07-wec01-domain-membership-confirmed.png` — WEC01 Server Manager showing domain `corp.local` and Ethernet `10.10.10.30`
- `screenshots/milestone07-wec01-domain-dc-network-validation.png` — WEC01 PowerShell validation showing `nltest /dsgetdc:corp.local`, static IP `10.10.10.30`, DNS `10.10.10.10`, and primary DNS suffix `corp.local`
- `screenshots/milestone07-test-win10-lan1-domain-dns-validated.png` — TEST-WIN10-LAN1 domain login, domain membership, DHCP, DNS, and `corp.local` resolution validated
- `screenshots/milestone07-test-win10-lan1-wec01-winrm-5985-reachable.png` — TEST-WIN10-LAN1 WinRM TCP `5985` reachability to `WEC01.corp.local` validated

## Milestone 6 Completion Criteria

Milestone 6 is complete only when:

- pfSense forwards logs to `SIEM-SPLUNK01`.
- Splunk displays pfSense logs.
- `TEST-WIN10-LAN1` sends logs to Splunk.
- Splunk displays Windows logs.
- Documentation includes source IPs, ports, screenshots, and validation searches.

## Current Status Summary

P1-2 is entering Milestone 8 — Sysmon Deployment and Local Validation.

Milestone 6 is complete. pfSense syslog (901+ events, host=10.10.10.1, UDP 5514) and Windows Event Log forwarding (WinEventLog:Security, host=DESKTOP-8K5AHHR, TCP 9997) are both validated in Splunk. Milestone 7 is complete. The collector placement decision has been made: a dedicated `WEC01` VM was provisioned (VMID 107, Windows Server 2022, 10.10.10.30) to keep the collector role separate from `AD-DC01`, matching production SOC practice. `AD-DC01` has been promoted to domain controller and the `corp.local` domain created. `WEC01` is joined to `corp.local`, and `TEST-WIN10-LAN1` is now domain-joined, using `AD-DC01` for domain DNS, able to resolve `WEC01.corp.local`, and able to reach `WEC01` on WinRM TCP `5985`. The next step is Milestone 8: deploy Sysmon and validate local Sysmon events on `TEST-WIN10-LAN1`.
