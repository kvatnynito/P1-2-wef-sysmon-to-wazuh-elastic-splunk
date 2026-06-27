# Validation Tests
Status: Draft - Milestone 8 local validation planned

## Goal
Confirm test activity appears at each stage of the pipeline as that stage is built. Milestone 8 starts with local endpoint validation before any collector or downstream platform validation.

## Milestone 8 local validation

Confirm controlled test activity appears on `TEST-WIN10-LAN1` in:
- Local Event Viewer
- `Microsoft-Windows-Sysmon/Operational`

Candidate local checks:
- Sysmon service running
- Sysmon process creation event visible locally
- Sysmon DNS or network event visible locally, if enabled by the selected config

## Later end-to-end validation

Confirm the same test activity appears across:
- Collector (a Windows machine that receives forwarded logs from multiple endpoints) (Event Viewer)
- Wazuh (an open-source security platform that collects agent data, generates alerts, and supports endpoint monitoring)
- Elastic (the Elasticsearch and Kibana stack — a search and visualization platform used to store and query log data)
- Splunk

## Test cases to run later
- Failed logon / brute force simulation
- Suspicious PowerShell execution
- Basic network connection telemetry (Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) Event ID 3)

## Evidence to add
- Screenshot per platform per test case
- Timestamp alignment notes (time sync / timezone)
- Quick “expected vs observed” notes
