# Splunk Free Ingestion
Status: Milestone 6 complete

## Goals
- Ingest pfSense (an open-source firewall and router acting as the network gateway in this lab) syslog (a standard format that network devices like firewalls use to send log messages) into Splunk Free
- Ingest Windows Event Logs into Splunk Free
- Validate SPL (Splunk Processing Language — the search syntax used to query Splunk) searches that prove log flow before detection work begins

## Implementation notes to add
- Windows ingestion method and forwarder settings
- Windows index and sourcetype choices
- Notes on troubleshooting if Windows forwarding fails

## Milestone 6 pfSense Syslog Input

Status: Validated

| Setting | Value |
|---|---|
| Source | `FW-EDGE01` / pfSense |
| Destination | `SIEM-SPLUNK01` |
| Input type | UDP (User Datagram Protocol — a fast, connectionless way to send data over a network, common for log delivery) |
| Port | `5514` |
| Source type | `syslog` (sourcetype — a label Splunk uses to identify what kind of log data came in, so it knows how to parse it) |
| App context | `search` |
| Host method | IP address of the remote sender |
| Index | `default` / `main` (the storage bucket Splunk uses to organize incoming log data) |

This input receives pfSense syslog from `FW-EDGE01`. Log flow was validated after pfSense remote logging was configured to send to `10.10.10.20:5514`.

## Milestone 6 pfSense Validation

Validation search:

```spl
index=main sourcetype=syslog
```

Validation result:

- 901 events returned in Splunk.
- `host=10.10.10.1`
- `source=udp:5514`
- `sourcetype=syslog`
- `filterlog` (pfSense's built-in logging format for firewall rule activity) entries visible.

Evidence:

- `screenshots/milestone06-splunk-udp5514-input-configured.png`

![Splunk UDP 5514 input created successfully](../screenshots/milestone06-splunk-udp5514-input-configured.png)

- `screenshots/milestone06-pfsense-remote-logging-configured-udp5514.png`

![pfSense remote logging configured to 10.10.10.20:5514](../screenshots/milestone06-pfsense-remote-logging-configured-udp5514.png)

- `screenshots/milestone06-splunk-pfsense-events-visible.png`

![Splunk search returning 901 pfSense syslog events](../screenshots/milestone06-splunk-pfsense-events-visible.png)

## Milestone 6 Windows Universal Forwarder

Status: Validated

| Setting | Value |
|---|---|
| Source | `TEST-WIN10-LAN1` |
| Destination | `SIEM-SPLUNK01` |
| Method | Splunk Universal Forwarder (a lightweight agent installed on an endpoint that ships its logs to a central SIEM) |
| Port | TCP (Transmission Control Protocol — a reliable, connection-based way to send data that confirms delivery) `9997` |
| Source type | `WinEventLog:Security`, `WinEventLog:Application`, `WinEventLog:System` |
| Index | `default` / `main` |

`inputs.conf` (a Splunk forwarder configuration file that defines which logs to collect and send) manually created on `TEST-WIN10-LAN1` with Application, Security, and System event channels enabled. Splunk TCP `9997` receiving port enabled on `SIEM-SPLUNK01`.

## Milestone 6 Windows Validation

Validation search:

```spl
index=* sourcetype=WinEventLog*
```

Validation result:

- 17 events returned in Splunk.
- `host=DESKTOP-8K5AHHR`
- `sourcetype=WinEventLog:Security`

Evidence:

- `screenshots/milestone06-splunk-tcp9997-receiving-enabled.png`

![Splunk TCP 9997 receiving port enabled](../screenshots/milestone06-splunk-tcp9997-receiving-enabled.png)

- `screenshots/milestone06-splunk-windows-events-visible.png`

![Splunk search returning Windows Security events from TEST-WIN10-LAN1](../screenshots/milestone06-splunk-windows-events-visible.png)
