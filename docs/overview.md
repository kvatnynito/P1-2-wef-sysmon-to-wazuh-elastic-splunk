# Project Overview
Status: Active - Milestone 8 in progress

## What this project builds
- Centralized Windows telemetry (the stream of log and event data collected from systems to support monitoring and investigation) pipeline using WEF (Windows Event Forwarding — a built-in Windows feature that pushes event logs from endpoint machines to a central collector) + Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections)
- Downstream ingestion (the process of receiving log data into a platform like Splunk) into Wazuh (an open-source security platform that collects agent data, generates alerts, and supports endpoint monitoring), Elastic (the Elasticsearch and Kibana stack — a search and visualization platform used to store and query log data), and Splunk Free for triage

## Scope
- First endpoint: `TEST-WIN10-LAN1`, joined to `corp.local`
- Future endpoints: AD-WIN10 / AD-WIN11
- Collector: dedicated `WEC01` (Windows Event Collector — the server-side role that receives forwarded Windows logs)
- Outputs: Wazuh, Elastic, Splunk Free

## Evidence to add
- Diagram link
- Screenshot links for each milestone
