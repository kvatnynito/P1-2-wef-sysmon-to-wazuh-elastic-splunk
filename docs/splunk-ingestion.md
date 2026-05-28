# Splunk Free Ingestion
Status: Milestone 6 in progress

## Goals
- Ingest pfSense syslog into Splunk Free
- Ingest Windows Event Logs into Splunk Free
- Validate SPL searches that prove log flow before detection work begins

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
| Input type | UDP |
| Port | `5514` |
| Source type | `syslog` |
| App context | `search` |
| Host method | IP address of the remote sender |
| Index | `default` / `main` |

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
- `filterlog` entries visible.

Evidence:

- `screenshots/milestone06-splunk-udp5514-input-configured.png`
- `screenshots/milestone06-pfsense-remote-logging-configured-udp5514.png`
- `screenshots/milestone06-splunk-pfsense-events-visible.png`

## Evidence to add

- Windows Event Log forwarding validation search.
- Windows Event Log events visible in Splunk.
- Notes on troubleshooting if any.
