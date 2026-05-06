# P1-2: Telemetry Pipeline

## Overview

This repo is planned to document how I will build a centralized Windows telemetry pipeline using **Windows Event Forwarding (WEF)** and **Sysmon**, then validate that the same telemetry is searchable in **Wazuh**, **Elastic**, and **Splunk**.

The goal is to practice realistic SOC workflows: collecting endpoint signals, confirming end-to-end delivery, and building repeatable investigation pivots across multiple platforms.

This project is part of Portfolio 1 and is planned as the next phase after `P1-1-proxmox-segmentation-lab`.

---

## Current Status

**Current status:** Planned  
**Execution status:** Not yet started  
**Prerequisite:** `P1-1-proxmox-segmentation-lab`

This repo is being prepared in advance so the pipeline design, host roles, validation steps, supporting hosts, and expected deliverables are already defined before implementation begins.

---

## Dependencies

This project is planned to be implemented on the segmented lab documented in:

- `P1-1-proxmox-segmentation-lab`

Planned hosts used during implementation:

- **Collector:** TBD (`AD-DC01` vs dedicated `WEC01`)
- **Endpoints:** `AD-WIN10` / `AD-WIN11`
- **Supporting hosts as needed:** `LAN1-FILE01`, `VULN-DVWA01`, `TEST-WIN2019-LAN2`, `SCAN-OPENVAS01`
- **Destinations:** Wazuh / Elastic / Splunk

---

## Sanitization Note

For safety, this repo uses **representative** hostnames and IP ranges and redacts any WAN/public IPs, domains/DDNS, VPN details, and secrets.

The architecture and workflow are intended to remain accurate, but specific identifiers may be modified.

---

## Why I’m Building This

I wanted a setup that mirrors how teams centralize endpoint telemetry in the real world:

- **WEF** to centralize Windows logs without installing heavy agents everywhere
- **Sysmon** to add high-value visibility such as process creation, network connections, image loads, and related endpoint behavior
- multiple destinations (**Wazuh / Elastic / Splunk**) so I can compare how each platform supports triage and investigation workflows

This repo is meant to help me practice not just collecting telemetry, but also validating that it moves end-to-end and becomes usable for analysis.

---

## Planned Project Goals

This project is intended to demonstrate:

- setting up **WEF (source-initiated subscription)** to forward Security and Sysmon events from endpoints to a collector
- deploying **Sysmon** with a tuned configuration on lab endpoints
- ingesting that telemetry into **Wazuh**, **Elastic**, and **Splunk**
- validating the pipeline using known test events
- capturing screenshots and notes that prove successful end-to-end delivery

---

## Network Diagram
![Network diagram](diagrams/proxmox-network-diagram.png)

**Design intent:** `FW-EDGE01` (pfSense) provides WAN access and acts as the central routing and segmentation point between **LAN1 (enterprise / blue-team)** and **LAN2 (vulnerable / testing)**.

---

## Architecture (High Level)

### Flow (Planned)
Endpoints (domain-joined Windows)  
→ **WEF Collector** (TBD: `AD-DC01` vs dedicated collector)  
→ **Wazuh / Elastic / Splunk**

---

## Why Each Component Is Included

- **WEF (Windows Event Forwarding)**  
  **Why it’s included:** I want a realistic and scalable way to centralize Windows events without depending entirely on one vendor’s collection method.

- **Sysmon**  
  **Why it’s included:** Sysmon adds the kind of visibility that makes investigations practical, especially around process activity, parent-child relationships, and network behavior.

- **Wazuh**  
  **Why it’s included:** I want experience with an open-source security platform that supports alerting, event review, and endpoint visibility.

- **Elastic**  
  **Why it’s included:** Elastic is useful for fast searching and field-based pivots, which makes it a strong platform for investigation workflows.

- **Splunk**  
  **Why it’s included:** Splunk is common in SOC environments, and I want hands-on repetition with SPL searches and triage patterns.

---

## Planned Workflow

Once implementation begins, the intended workflow for this repo is:

### 1. Decide collector placement
- choose between `AD-DC01` and a dedicated `WEC01`
- document the reasoning and design impact

### 2. Prepare supporting hosts
- finalize which additional hosts are needed for telemetry generation and validation
- add supporting hosts only when they serve a telemetry, validation, detection, or investigation purpose

### 3. Deploy Sysmon to endpoints
- install Sysmon on Windows lab endpoints
- apply a tuned configuration
- confirm event generation locally

### 4. Configure WEF
- create a source-initiated subscription
- forward selected Security and Sysmon events to the collector
- confirm event arrival in Event Viewer

### 5. Send telemetry to downstream platforms
- validate Wazuh ingestion
- validate Elastic ingestion
- validate Splunk ingestion

### 6. Run test activity and collect evidence
- generate known test events
- confirm visibility in each platform
- capture screenshots and notes
- document issues, gaps, and improvements

---

## Planned Deliverables

This repo is expected to eventually include:

- WEF configuration notes
- Sysmon deployment and configuration notes
- pipeline diagrams
- screenshots of collector-side event validation
- screenshots of Wazuh ingestion
- screenshots of Elastic ingestion
- screenshots of Splunk ingestion
- supporting host onboarding notes where relevant
- test-event validation notes
- documentation of investigation pivots and lessons learned

---

## Milestone Tracker

- [x] Milestone 1: Repo structure and telemetry design baseline
- [ ] Milestone 2: Decide collector placement and prepare first Windows endpoint
- [ ] Milestone 3: Deploy Sysmon and confirm local event generation
- [ ] Milestone 4: Configure WEF and confirm collector-side event receipt
- [ ] Milestone 5: Validate telemetry ingestion in Wazuh, Elastic, and Splunk
- [ ] Milestone 6: Add `LAN1-FILE01` and expand Windows telemetry coverage
- [ ] Milestone 7: Add `AD-WIN11` and expand endpoint coverage
- [ ] Milestone 8: Add `VULN-DVWA01` and validate web activity telemetry
- [ ] Milestone 9: Add `TEST-WIN2019-LAN2` and validate Windows server telemetry
- [ ] Milestone 10: Add `SCAN-OPENVAS01` and review scan-generated telemetry
- [ ] Milestone 11: Optional `VULN-WEBGOAT01` telemetry expansion
- [ ] Milestone 12: Optional `VULN-UBU-OLD` telemetry expansion

---

## Planned Next Steps

When work begins on this repo, the initial implementation focus will likely be:

- finalize collector placement
- prepare the first Windows endpoint for telemetry onboarding
- install Sysmon on the first Windows endpoint
- configure WEF forwarding
- validate collector-side log receipt
- document the first successful ingestion path
