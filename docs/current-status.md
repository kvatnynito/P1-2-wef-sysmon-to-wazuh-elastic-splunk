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

## Current Step

**Active step:** Step 10 — Multi-Platform Ingestion Validation (Splunk portion complete; Wazuh and Elastic not yet started — neither has a VM built yet)

## Completed P1-2 Steps

- Step 6 — Logging Foundation
- Step 7 — Collector Placement and First Endpoint Prep
- Step 8 — Sysmon Deployment and Local Validation
- Step 9 — WEF Configuration and Collector Validation

## Current Objective

Steps 6, 7, 8, and 9 are complete. Collector placement has been decided: a dedicated `WEC01` collector is in use. `TEST-WIN10-LAN1` has been joined to `corp.local`, confirmed ready for WEF and Sysmon onboarding, and Sysmon has been deployed with the SwiftOnSecurity config and validated locally. WEF is fully configured and validated: `WEC01` receives Security and Sysmon events from `TEST-WIN10-LAN1` in real time. Step 10 is now in progress: the Splunk half is done — a Universal Forwarder was installed on `WEC01` shipping its Forwarded Events log to `SIEM-SPLUNK01`, confirmed searchable (42 events for `host=WEC01`). Wazuh and Elastic still need their own VMs built from scratch before their ingestion can be validated — deliberately scoped as separate future sessions.

Step 8 covered:

- Deploying Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) to `TEST-WIN10-LAN1`
- Applying a controlled Sysmon configuration (SwiftOnSecurity baseline, schema 4.91)
- Generating basic local endpoint activity
- Confirming Sysmon events are visible locally — 24 events in the Operational log, including Event ID 1 (Process Create), Event ID 22 (DNS query), and Event ID 13 (Registry value set), on host `DESKTOP-8K5AHHR.corp.local`

Step 9 covered:

- Configuring `WEC01` as the WEF collector (`wecutil qc`)
- Configuring `TEST-WIN10-LAN1` as a WEF source (`winrm quickconfig`, GPO Subscription Manager pointed at `WEC01`, `gpupdate /force`)
- Creating a source-initiated subscription on `WEC01` covering both the Security log and the `Microsoft-Windows-Sysmon/Operational` log (added via manual XML edit, since Sysmon's log isn't registered locally on the collector and can't be picked from the standard log tree)
- Troubleshooting and fixing a real permissions gap: the NETWORK SERVICE account lacked read access to Sysmon's custom log channel, causing subscription creation to fail with error 5004 — fixed via `wevtutil sl Microsoft-Windows-Sysmon/Operational /ca:...` granting NETWORK SERVICE and Event Log Readers read access
- Diagnosing and fixing a second issue (an internal "file name is too long" failure tied to the combined length of the subscription name, channel name, and computer FQDN) by renaming the endpoint from its auto-generated Windows hostname `DESKTOP-8K5AHHR` to `TEST-WIN10-LAN1` (matching its Proxmox VM label) and recreating the subscription with a shorter name
- Validating forwarding end-to-end: 1,128 events confirmed in `WEC01`'s Forwarded Events log, sourced from `TEST-WIN10-LAN1.corp.local`

Step 9 is complete and validated. Step 10 (Wazuh/Elastic/Splunk ingestion validation) is next.

Step 10 so far (Splunk portion):

- Installed the Splunk Universal Forwarder on `WEC01`, configured to monitor its local `Forwarded Events` log (the log WEF writes Security + Sysmon events into) and ship it to `SIEM-SPLUNK01` on TCP `9997` — the same receiving port `TEST-WIN10-LAN1`'s forwarder already used successfully since Step 6
- Hit and fixed a real infrastructure issue unrelated to the forwarder itself: `SIEM-SPLUNK01`'s 60-day Enterprise Trial license (installed during P1-1, 2026-05-13) had expired, blocking all searches with a license-limit error. Fixed by switching the license group to the free Splunk license (500MB/day, sufficient for this lab) via Settings → Licensing, then restarting the Splunk service
- Validated ingestion end-to-end: search `host="WEC01"` returned 42 events in the last 15 minutes, sourcetype `WinEventLog:ForwardedEvents`, including real Sysmon Process Create events originating from `TEST-WIN10-LAN1.corp.local` — full pipeline (`TEST-WIN10-LAN1` → `WEC01` → `SIEM-SPLUNK01`) confirmed searchable in Splunk

Wazuh and Elastic ingestion remain not started — neither has a VM built yet, deliberately scoped as separate future work rather than rushed in this same session.

**Naming note:** `TEST-WIN10-LAN1` was renamed at the Windows/AD level partway through Step 9 — it was previously `DESKTOP-8K5AHHR` (an auto-generated name that was never changed at OS install time). All Step 6-8 evidence and the earlier part of Step 9 was captured under the old name; same machine throughout.

## Current Lab Systems Available from P1-1

| System | Role | Status |
|---|---|---|
| `FW-EDGE01` | pfSense firewall/router | Available from P1-1 |
| `AD-DC01` | Domain Controller / DNS | Available from P1-1 |
| `WEC01` | Dedicated Windows Event Collector | Added during P1-2 |
| `TEST-WIN10-LAN1` | First Windows endpoint / future AD-WIN10 | Available from P1-1 |
| `TEST-WIN10-LAN2` | LAN2 Windows endpoint | Available from P1-1 |
| `ATTACK-KALI01` | Kali traffic generator | Available from P1-1 |
| `VULN-METASPLOITABLE2` | Vulnerable Linux target | Available from P1-1 |
| `SIEM-SPLUNK01` | Splunk destination | Available from P1-1 |

## Step 6 Log Source Map

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
- Step 6 complete — both initial log sources validated in Splunk.
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
- Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) has been deployed to `TEST-WIN10-LAN1` with the SwiftOnSecurity config and validated locally — 24 events in the Operational log including Event ID 1 (Process Create).
- WEF has not been configured.
- Wazuh (an open-source security platform that collects agent data, generates alerts, and supports endpoint monitoring) ingestion has not been validated.
- Elastic (the Elasticsearch and Kibana stack — a search and visualization platform used to store and query log data) ingestion has not been validated.

## Current Rule

Do not begin WEF configuration until Sysmon has been deployed and validated locally on the first endpoint. **Satisfied 2026-08-06** — Sysmon is deployed and validated on `TEST-WIN10-LAN1`; WEF subscription work (Step 9) may now begin.

## Next Actions

1. ~~Download or stage Sysmon and the selected Sysmon configuration on `TEST-WIN10-LAN1`.~~ Done.
2. ~~Install Sysmon on `TEST-WIN10-LAN1`.~~ Done — `Sysmon64` installed and started with SwiftOnSecurity config (schema 4.91) via `-accepteula -i sysmonconfig-export.xml`.
3. ~~Generate basic local endpoint activity.~~ Done — Notepad and Command Prompt opened on TEST-WIN10-LAN1.
4. ~~Confirm Sysmon service state and local Sysmon events in Event Viewer.~~ Done — 24 events in Sysmon Operational log, including Event ID 1 (Process Create), 22 (DNS query), 13 (Registry value set).
5. ~~Document Step 8 validation before starting WEF subscriptions.~~ Done — this file. Step 8 is complete; next step is WEF subscription configuration (Step 9).

## Evidence Captured

- `screenshots/step06-siem-splunk01-ip-confirmed.png`
- `screenshots/step06-splunk-service-running.png`
- `screenshots/step06-splunk-web-ui-reachable-from-win10.png`
- `screenshots/step06-splunk-udp5514-input-configured.png`
- `screenshots/step06-pfsense-remote-logging-configured-udp5514.png`
- `screenshots/step06-splunk-pfsense-events-visible.png` — pfSense validation, 901 events
- `screenshots/step06-splunk-tcp9997-receiving-enabled.png` — Splunk TCP 9997 receiving port configured and enabled
- `screenshots/step06-splunk-windows-events-visible.png` — Windows Event Log validation, WinEventLog:Security events confirmed from DESKTOP-8K5AHHR
- `screenshots/step07-wec01-domain-membership-confirmed.png` — WEC01 Server Manager showing domain `corp.local` and Ethernet `10.10.10.30`
- `screenshots/step07-wec01-domain-dc-network-validation.png` — WEC01 PowerShell validation showing `nltest /dsgetdc:corp.local`, static IP `10.10.10.30`, DNS `10.10.10.10`, and primary DNS suffix `corp.local`
- `screenshots/step07-test-win10-lan1-domain-dns-validated.png` — TEST-WIN10-LAN1 domain login, domain membership, DHCP, DNS, and `corp.local` resolution validated
- `screenshots/step07-test-win10-lan1-wec01-winrm-5985-reachable.png` — TEST-WIN10-LAN1 WinRM TCP `5985` reachability to `WEC01.corp.local` validated
- `screenshots/step08-sysmon-package-downloaded-extracted.png` — Sysmon zip downloaded on TEST-WIN10-LAN1, contents previewed via Explorer's built-in zip browser (Eula, Sysmon, Sysmon64, Sysmon64a visible) — not yet extracted to a real folder at this point
- `screenshots/step08-sysmon-zip-actually-extracted.png` — Sysmon zip properly extracted via Extract All to a real folder at `C:\Users\Administrator\Downloads\Sysmon`; `Sysmon.exe`/`Sysmon64.exe`/`Sysmon64a.exe` now show as real Application files, confirming the package is genuinely staged for install
- `screenshots/step08-sysmon-config-verified-xml.png` — `sysmonconfig-export` now shows Type `XML File` (121 KB) in the same folder, confirming the SwiftOnSecurity config saved correctly as real XML after earlier HTML-save mistakes were corrected — both Sysmon and its config are fully staged and ready for install
- `screenshots/step08-sysmon64-install-success.png` — `Sysmon64.exe -accepteula -i sysmonconfig-export.xml` run on TEST-WIN10-LAN1; console output confirms config schema 4.91 validated, `Sysmon64 installed`, `SysmonDrv installed`, `SysmonDrv started`, `Sysmon64 started`
- `screenshots/step08-local-activity-generated.png` — Notepad and an elevated Command Prompt opened on TEST-WIN10-LAN1 to generate local process-creation activity for Sysmon to capture
- `screenshots/step08-sysmon-operational-log-events-confirmed.png` — Event Viewer, Microsoft-Windows-Sysmon/Operational log on TEST-WIN10-LAN1 (host `DESKTOP-8K5AHHR.corp.local`) showing 24 events: Event ID 1 (Process Create), Event ID 22 (DNS query), Event ID 13 (Registry value set) — Sysmon local validation complete, Step 8 done
- `screenshots/step09-winrm-quickconfig-success.png` — `winrm quickconfig` run on TEST-WIN10-LAN1: WinRM service started and set to delayed auto-start, firewall exception enabled, "WinRM has been updated for remote management"
- `screenshots/step09-subscription-manager-value-entered.png` — Local Group Policy Editor on TEST-WIN10-LAN1, Computer Configuration → Administrative Templates → Windows Components → Event Forwarding → Configure target Subscription Manager, set to Enabled with value `Server=http://WEC01.corp.local:5985/wsman/SubscriptionManager/WEC,Refresh=60`
- `screenshots/step09-gpupdate-force-success.png` — `gpupdate /force` run on TEST-WIN10-LAN1 after applying the Subscription Manager policy; "Computer Policy update has completed successfully. User Policy update has completed successfully."
- `screenshots/step09-subscription-created-active.png` — WEC01 Event Viewer, Subscriptions node: `TEST-WIN10-LAN1-Security-Sysmon` subscription created, Status Active, Type Source Initiated, source computer `CORP\DESKTOP-8K5AHHR`, events query covering Security and Microsoft-Windows-Sysmon/Operational (added via manual XML edit since Sysmon's log isn't registered locally on the collector), Destination Log Forwarded Events
- `screenshots/step09-subscription-runtime-status-active.png` — Subscription Runtime Status on WEC01: "Active - : No additional status" for both the subscription and source computer `DESKTOP-8K5AHHR.corp.local` (1 Total, 1 Active) — confirms the WinRM connection from the endpoint to the collector succeeded
- `screenshots/step09-hostname-renamed-test-win10-lan1.png` — `hostname` on the endpoint now returns `TEST-WIN10-LAN1` (renamed from the auto-generated `DESKTOP-8K5AHHR`, matching the Proxmox VM label); `whoami` confirms domain trust survived the rename and reboot (`corp\administrator`). **Note for continuity:** all Step 6-8 evidence and the early Step 9 troubleshooting above were captured while this machine was still named `DESKTOP-8K5AHHR` — same machine throughout, name changed partway through Step 9
- `screenshots/step09-forwarded-events-1128-confirmed.png` — `WEC01` Forwarded Events log showing 1,128 events from `TEST-WIN10-LAN1.corp.local` after recreating the subscription against the renamed host; selected Event ID 13 (Registry value set, Sysmon rule `Suspicious.ImageBeginWithBackslash`) confirms real Sysmon telemetry is flowing end-to-end — Step 9 complete
- `screenshots/step10-splunk-host-wec01-42-events.png` — Splunk search `host="WEC01"`, last 15 minutes, returning 42 events sourcetype `WinEventLog:ForwardedEvents`; top event is a real Sysmon Process Create (EventCode=1) originally from `TEST-WIN10-LAN1.corp.local` — confirms the full `TEST-WIN10-LAN1` → `WEC01` → `SIEM-SPLUNK01` pipeline is searchable in Splunk
- `screenshots/step10-splunk-live-test-whoami-confirmed.png` — Deliberate live end-to-end test: ran `whoami` on `TEST-WIN10-LAN1`, located it in Splunk via `host="WEC01" CommandLine=whoami`, confirmed `Image=C:\Windows\System32\whoami.exe` from `TEST-WIN10-LAN1.corp.local` — proves the full pipeline in near-real time, not just historical backlog. Also surfaced and diagnosed a real display-only issue along the way: Splunk's Time column showed timestamps ~7 hours ahead of local time because the forwarding host (`WEC01`) has its system clock set to UTC rather than Arizona time — confirmed harmless (the raw Windows event and Sysmon's own `UtcTime` field were both internally consistent) by comparing against Sysmon's embedded `UtcTime` field
- `screenshots/step10-duplicate-security-forwarding-removed.png` — `net stop`/`net start SplunkForwarder` on `TEST-WIN10-LAN1` after editing its `inputs.conf` to set `disabled = 1` under `[WinEventLog://Security]` only (Application and System left untouched); removes duplicate Security log ingestion now that Security data also arrives via the `WEC01`/WEF pipeline
- **Full architecture cleanup:** rather than leave a hybrid setup (Security+Sysmon via `WEC01`, Application+System via `TEST-WIN10-LAN1`'s own direct forwarder), completed the cutover so `TEST-WIN10-LAN1` needs no direct Splunk forwarder at all — all four channels now flow exclusively through `WEC01`. Set all three stanzas (`Application`, `Security`, `System`) to `disabled = 1` in `TEST-WIN10-LAN1`'s `inputs.conf`. Attempted to add Application + System to the existing `TEST-WIN10-Security-Sysmon` subscription's query, which re-triggered the same "file name is too long" bookmark-path issue from Step 9 (confirmed via `WEC01`'s local Forwarded Events log, Event 111, `Microsoft-Windows-EventForwarder`) — now that the subscription tracked 4 channels instead of 2, the already-shortened name wasn't short enough. Fixed by creating a **second, separate subscription** (short name, Application + System only, picked via the standard checkbox tree since — unlike Sysmon — these are logs `WEC01` already knows about locally) rather than risking the already-working first subscription. Once both subscriptions were confirmed delivering real data, fully uninstalled the Splunk Universal Forwarder from `TEST-WIN10-LAN1` (Apps & Features) — the endpoint now runs no third-party forwarding software at all; every log channel reaches Splunk exclusively through `WEC01`.
- `screenshots/step10-application-log-via-wec01-confirmed.png` — Splunk search `host=wec01 LogName=Application`, all time, 5 events including the original test entry (`EventCode=9999`) — confirms the second subscription is delivering Application events
- `screenshots/step10-system-log-via-wec01-confirmed.png` — Splunk search `host=wec01 LogName=System`, all time, 7 events including a real Group Policy processing event (`EventCode=1501`) — confirms System is flowing too. **All four channels (Security, Sysmon, Application, System) now flow exclusively through `WEC01` via two subscriptions; `TEST-WIN10-LAN1` no longer sends anything to Splunk directly.**

## Step 6 Completion Criteria

Step 6 is complete only when:

- pfSense forwards logs to `SIEM-SPLUNK01`.
- Splunk displays pfSense logs.
- `TEST-WIN10-LAN1` sends logs to Splunk.
- Splunk displays Windows logs.
- Documentation includes source IPs, ports, screenshots, and validation searches.

## Current Status Summary

P1-2 has completed Step 8 — Sysmon Deployment and Local Validation.

Step 6 is complete. pfSense syslog (901+ events, host=10.10.10.1, UDP 5514) and Windows Event Log forwarding (WinEventLog:Security, host=DESKTOP-8K5AHHR, TCP 9997) are both validated in Splunk. Step 7 is complete. The collector placement decision has been made: a dedicated `WEC01` VM was provisioned (VMID 107, Windows Server 2022, 10.10.10.30) to keep the collector role separate from `AD-DC01`, matching production SOC practice. `AD-DC01` has been promoted to domain controller and the `corp.local` domain created. `WEC01` is joined to `corp.local`, and `TEST-WIN10-LAN1` is now domain-joined, using `AD-DC01` for domain DNS, able to resolve `WEC01.corp.local`, and able to reach `WEC01` on WinRM TCP `5985`. Step 8 is now complete: Sysmon64 was deployed on `TEST-WIN10-LAN1` with the SwiftOnSecurity config (schema 4.91) and validated locally — 24 events in the Sysmon Operational log, including Event ID 1 (Process Create), Event ID 22 (DNS query), and Event ID 13 (Registry value set). The next step is Step 9: configure WEF subscriptions to forward these events from `TEST-WIN10-LAN1` to `WEC01`.
