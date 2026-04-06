https://github.com/kvatnynito/Cybersecurity-Portfolio1

# P1-2: Telemetry Pipeline 

## Overview
This project documents how I’m building a centralized Windows telemetry pipeline using **Windows Event Forwarding (WEF)** and **Sysmon**, then validating that the same telemetry is searchable in **Wazuh**, **Elastic**, and **Splunk**.

The goal is to practice realistic SOC workflows: collecting endpoint signals, confirming end-to-end delivery, and building repeatable investigation pivots.

## Dependencies
This project is implemented on the segmented lab documented in:
- P1-1-proxmox-segmentation-lab

Hosts used (TBD during implementation):
- Collector: TBD (AD-DC01 vs WEC01)
- Endpoints: AD-WIN10 / AD-WIN11
- Destinations: Wazuh / Elastic / Splunk Free

## Sanitization Note
For safety, this repo uses **representative** hostnames/IP ranges and redacts any WAN/public IPs, domains/DDNS, VPN details, and secrets.  
The architecture and workflow are accurate, but specific identifiers may be modified.

## Why I’m building this
I wanted a setup that mirrors how teams centralize endpoint telemetry in the real world:
- **WEF** to centralize Windows logs without installing heavy agents everywhere
- **Sysmon** to add high-value visibility (process creation, network connections, image loads, etc.)
- Multiple destinations (**Wazuh / Elastic / Splunk**) so I can compare how each platform supports triage and investigations

## Project Goals
- Set up **WEF (source-initiated subscription)** to forward Security + Sysmon events from endpoints to a collector
- Deploy **Sysmon** with a tuned configuration on lab endpoints
- Ingest the telemetry into **Wazuh**, **Elastic**, and **Splunk**
- Validate the pipeline using known test events and capture evidence screenshots

## Architecture (High Level)
**Flow (planned):**
Endpoints (domain-joined Windows)  
→ **WEF Collector** (TBD: AD-DC01 vs dedicated collector)  
→ **Wazuh / Elastic / Splunk**

Diagram (planned): `diagrams/pipeline-diagram.png`  
(See checklist: `diagrams/README.md`)

## Why each component is included
- **WEF (Windows Event Forwarding)**  
  **Why it’s included:** I want a realistic, scalable way to centralize Windows events without relying on one vendor’s agent.

- **Sysmon**  
  **Why it’s included:** Sysmon adds visibility that makes investigations practical—especially process/network relationships that don’t always show up cleanly in default logs.

- **Wazuh**  
  **Why it’s included:** I want experience with an open-source, agent-based security platform that supports alerting and endpoint visibility.

- **Elastic**  
  **Why it’s included:** Elastic is great for fast searching and field-based pivots, so it’s a strong platform to practice “find → filter → pivot” investigation workflows.

- **Splunk**  
  **Why it’s included:** Splunk is common in SOC environments, and I want hands-on repetition with SPL searches and investigation patterns.

## Milestones (Blueprint → Implementation)
- [x] Repo structure created (docs/diagrams/screenshots)
- [ ] Decide collector placement (AD-DC01 vs dedicated WEC host)
- [ ] Install Sysmon on endpoints + confirm event generation
- [ ] Configure WEF subscription (source-initiated)
- [ ] Confirm events arrive at collector (Event Viewer)
- [ ] Confirm Wazuh ingestion (screenshot evidence)
- [ ] Confirm Elastic ingestion (screenshot evidence)
- [ ] Confirm Splunk ingestion (screenshot evidence)
- [ ] Run validation tests + document results

