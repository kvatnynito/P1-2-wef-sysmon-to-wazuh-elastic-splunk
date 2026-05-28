# Validation Tests (End-to-End)
Status: Draft (to be updated during implementation)

## Goal
Confirm the same test activity appears across:
- Collector (a Windows machine that receives forwarded logs from multiple endpoints) (Event Viewer)
- Wazuh (an open-source security platform that collects agent data, generates alerts, and supports endpoint monitoring)
- Elastic (the Elasticsearch and Kibana stack — a search and visualization platform used to store and query log data)
- Splunk

## Test cases to run
- Failed logon / brute force simulation
- Suspicious PowerShell execution
- Basic network connection telemetry (Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) Event ID 3)

## Evidence to add
- Screenshot per platform per test case
- Timestamp alignment notes (time sync / timezone)
- Quick “expected vs observed” notes
