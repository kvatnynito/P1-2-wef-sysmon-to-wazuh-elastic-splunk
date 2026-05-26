# Splunk Free Ingestion
Status: Draft (to be updated during implementation)

## Goals
- Ingest Windows + Sysmon events into Splunk Free
- Validate SPL searches for common investigations

## Implementation notes to add
- Ingestion method (Universal Forwarder on collector vs endpoint)
- Inputs configuration approach (sanitized)
- Index + sourcetype choices

## Milestone 6 pfSense Syslog Input

Status: Configured, not yet validated

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

This input prepares Splunk to receive pfSense syslog. It does not prove log flow until pfSense is configured to forward logs and a Splunk search returns pfSense events.

## Evidence to add
- Screenshot: Data arriving (Search head)
- 2–3 core SPL queries used for validation
- Notes on troubleshooting (if any)
