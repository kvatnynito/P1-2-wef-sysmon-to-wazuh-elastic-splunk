# Milestone Schedule

## Purpose

This file is the roadmap source of truth for the Telemetry Pipeline project.

Use this file to track:

- milestone order
- milestone goals
- telemetry onboarding sequence
- supporting host additions
- completion criteria
- portfolio artifact ideas
- validation checkpoints
- when additional telemetry-supporting VMs should be added

This file shows the intended project roadmap for P1-2 only. For the current live stopping point, use:

- `docs/current-status.md`

---

# Current Project Position

## Current Milestone

- Milestone 6 — Collector Placement and First Endpoint Prep

## Completed Milestones

- None yet.

## Current Note

P1-2 begins after P1-1 is completed.

Milestones 1–5 belong to `P1-1-proxmox-segmentation-lab` and cover the segmented lab foundation.

The segmented lab foundation already exists in `P1-1-proxmox-segmentation-lab`, including:

- `FW-EDGE01`
- `AD-DC01`
- `TEST-WIN10-LAN1`
- `TEST-WIN10-LAN2`
- `ATTACK-KALI01`
- `VULN-METASPLOITABLE2`
- `SIEM-SPLUNK01`

This repo focuses on telemetry collection, forwarding, validation, and investigation readiness.

Repo structure for P1-2 has already been created, but implementation has not started yet.

The first actionable milestone in this repo is Milestone 6.

Additional hosts may be created during this project, but only when they support telemetry, detection, or investigation workflows.

---

## Milestone 6 — Collector Placement and First Endpoint Prep

### Goal

Decide collector placement and prepare the first Windows endpoint for telemetry onboarding.

### Tasks

- Decide whether the collector will be `AD-DC01` or a dedicated `WEC01`.
- Document reasoning and design tradeoffs.
- Confirm first endpoint selection.
- Prepare the first Windows endpoint for telemetry onboarding.
- Confirm the endpoint is reachable and ready for logging configuration.

### Expected Result

- Collector placement decision is documented.
- First endpoint is chosen and ready.
- The initial telemetry path has a defined starting point.

### Portfolio Artifact Ideas

- `docs/milestone-06-collector-placement-decision.md`
- `docs/milestone-06-first-endpoint-prep.md`

### Status

Planned.

---

## Milestone 7 — Sysmon Deployment and Local Validation

### Goal

Deploy Sysmon and confirm local event generation on the first Windows endpoint.

### Tasks

- Install Sysmon on the first Windows endpoint.
- Apply a tuned configuration.
- Confirm Sysmon service is running.
- Generate basic local activity.
- Confirm Sysmon events are visible locally.
- Document the configuration and validation evidence.

### Completion Criteria

Milestone 7 is complete only when:

- Sysmon is installed.
- Sysmon is running.
- Local Sysmon events are generated.
- Validation evidence is documented.

### Portfolio Artifact Ideas

- `docs/milestone-07-sysmon-deployment.md`
- `docs/milestone-07-sysmon-local-validation.md`

### Status

Planned.

---

## Milestone 8 — WEF Configuration and Collector Validation

### Goal

Configure WEF and confirm event receipt at the collector.

### Tasks

- Configure a source-initiated WEF subscription.
- Forward selected Security and Sysmon events.
- Confirm event arrival on the collector.
- Document subscription choices.
- Save screenshots and validation notes.

### Completion Criteria

Milestone 8 is complete only when:

- WEF subscription is configured.
- Security and Sysmon events arrive at the collector.
- Validation evidence is documented.

### Portfolio Artifact Ideas

- `docs/milestone-08-wef-subscription-setup.md`
- `docs/milestone-08-collector-event-validation.md`

### Status

Planned.

---

## Milestone 9 — Multi-Platform Ingestion Validation

### Goal

Validate telemetry ingestion in Wazuh, Elastic, and Splunk.

### Tasks

- Confirm telemetry reaches Wazuh.
- Confirm telemetry reaches Elastic.
- Confirm telemetry reaches Splunk.
- Run test activity and verify searchability in each platform.
- Capture screenshots and validation notes.

### Completion Criteria

Milestone 9 is complete only when:

- Telemetry is searchable in Wazuh.
- Telemetry is searchable in Elastic.
- Telemetry is searchable in Splunk.
- Validation evidence is documented.

### Portfolio Artifact Ideas

- `docs/milestone-09-wazuh-ingestion-validation.md`
- `docs/milestone-09-elastic-ingestion-validation.md`
- `docs/milestone-09-splunk-ingestion-validation.md`

### Status

Planned.

---

## Milestone 10 — File Server Telemetry Expansion

### Goal

Add `LAN1-FILE01` and expand Windows telemetry coverage.

### Tasks

- Create `LAN1-FILE01`.
- Connect `LAN1-FILE01` to LAN1 / vmbr1.
- Configure static IP.
- Join the system to the domain if appropriate.
- Create a basic SMB file share.
- Generate file activity and authentication activity.
- Onboard the host into the telemetry pipeline.
- Validate visibility in the collector and downstream platforms.
- Document findings.

### Add This VM

| VM Name | Role | Network | Status |
|---|---|---|---|
| LAN1-FILE01 | File server / SMB telemetry source | LAN1 / vmbr1 | Planned Milestone 10 |

### Why This Matters

This host creates realistic enterprise telemetry such as SMB activity, authentication events, file operations, and permissions-related behavior.

### Portfolio Artifact Ideas

- `docs/milestone-10-file-server-onboarding.md`
- `docs/milestone-10-smb-telemetry-validation.md`

### Status

Planned.

---

## Milestone 11 — Second Endpoint Telemetry Expansion

### Goal

Add `AD-WIN11` and expand endpoint telemetry coverage.

### Tasks

- Create `AD-WIN11`.
- Connect `AD-WIN11` to LAN1 / vmbr1.
- Join `AD-WIN11` to the domain.
- Onboard the endpoint into the telemetry pipeline.
- Generate endpoint activity:
  - logon
  - failed logon
  - PowerShell usage
  - file share access
- Validate visibility in the collector and downstream platforms.
- Document endpoint onboarding.

### Add This VM

| VM Name | Role | Network | Status |
|---|---|---|---|
| AD-WIN11 | Second domain-joined Windows telemetry endpoint | LAN1 / vmbr1 | Planned Milestone 11 |

### Portfolio Artifact Ideas

- `docs/milestone-11-ad-win11-onboarding.md`
- `docs/milestone-11-endpoint-telemetry-validation.md`

### Status

Planned.

---

## Milestone 12 — DVWA Telemetry Source

### Goal

Add `VULN-DVWA01` and validate web activity telemetry.

### Tasks

- Create `VULN-DVWA01`.
- Connect `VULN-DVWA01` to LAN2 / vmbr2.
- Install Ubuntu Server.
- Install Docker.
- Deploy DVWA.
- Confirm DVWA is reachable from `ATTACK-KALI01`.
- Generate basic web traffic.
- Validate relevant firewall, IDS, and SIEM visibility.
- Document setup and findings.

### Add This VM

| VM Name | Role | Network | Status |
|---|---|---|---|
| VULN-DVWA01 | Vulnerable web telemetry source | LAN2 / vmbr2 | Planned Milestone 12 |

### Portfolio Artifact Ideas

- `docs/milestone-12-dvwa-setup.md`
- `docs/milestone-12-dvwa-telemetry-validation.md`

### Status

Planned.

---

## Milestone 13 — Windows Server Telemetry Source

### Goal

Add `TEST-WIN2019-LAN2` and validate Windows Server telemetry.

### Tasks

- Create `TEST-WIN2019-LAN2`.
- Connect `TEST-WIN2019-LAN2` to LAN2 / vmbr2.
- Install Windows Server 2019 evaluation.
- Configure static IP.
- Enable selected services for testing.
- Onboard the server into the telemetry pipeline as appropriate.
- Generate Windows Server activity.
- Validate telemetry visibility.
- Document baseline configuration.

### Naming Rule

Name it first as `TEST-WIN2019-LAN2`.

Later, when intentionally weakened, document it as `VULN-WIN2019`.

Do not call it vulnerable before it is actually misconfigured.

### Add This VM

| VM Name | Role | Network | Status |
|---|---|---|---|
| TEST-WIN2019-LAN2 | Windows Server telemetry source | LAN2 / vmbr2 | Planned Milestone 13 |
| VULN-WIN2019 | Intentionally weakened Windows Server target | LAN2 / vmbr2 | Future state after misconfiguration |

### Portfolio Artifact Ideas

- `docs/milestone-13-windows-server-onboarding.md`
- `docs/milestone-13-windows-server-telemetry-validation.md`

### Status

Planned.

---

## Milestone 14 — Vulnerability Scan Telemetry

### Goal

Add `SCAN-OPENVAS01` and review scan-generated telemetry.

### Tasks

- Create `SCAN-OPENVAS01`.
- Connect `SCAN-OPENVAS01` to LAN2 / vmbr2.
- Install Greenbone/OpenVAS.
- Configure scanner.
- Run scans against selected targets.
- Review firewall, IDS, and SIEM telemetry generated by scanning.
- Export or screenshot scan results.
- Document findings.

### Add This VM

| VM Name | Role | Network | Status |
|---|---|---|---|
| SCAN-OPENVAS01 | Vulnerability scanner / telemetry generator | LAN2 / vmbr2 | Planned Milestone 14 |

### Naming Note

Use `SCAN-OPENVAS01`, not `VULN-OPENVAS01`.

OpenVAS is the scanner, not the victim.

### Portfolio Artifact Ideas

- `docs/milestone-14-openvas-setup.md`
- `docs/milestone-14-scan-telemetry-review.md`

### Status

Planned.

---

## Milestone 15 — Optional WebGoat Telemetry Expansion

### Goal

Add `VULN-WEBGOAT01` as an optional second vulnerable web telemetry source.

### Tasks

- Create `VULN-WEBGOAT01`.
- Connect `VULN-WEBGOAT01` to LAN2 / vmbr2.
- Install Ubuntu Server.
- Install Docker.
- Deploy WebGoat.
- Confirm access from `ATTACK-KALI01`.
- Generate basic web traffic.
- Validate relevant telemetry.
- Document setup.

### Add This VM

| VM Name | Role | Network | Status |
|---|---|---|---|
| VULN-WEBGOAT01 | Optional vulnerable web telemetry source | LAN2 / vmbr2 | Optional Milestone 15 |

### Status

Optional / Planned.

---

## Milestone 16 — Optional Outdated Linux Telemetry Expansion

### Goal

Add `VULN-UBU-OLD` as an optional outdated Linux telemetry source.

### Tasks

- Create `VULN-UBU-OLD`.
- Connect `VULN-UBU-OLD` to LAN2 / vmbr2.
- Install older Ubuntu version.
- Configure selected services.
- Avoid full hardening.
- Run scans or test activity against it.
- Review resulting telemetry.
- Document findings.

### Add This VM

| VM Name | Role | Network | Status |
|---|---|---|---|
| VULN-UBU-OLD | Optional outdated Linux telemetry source | LAN2 / vmbr2 | Optional Milestone 16 |

### Status

Optional / Planned.

---

# Milestone Tracker Reference

Use the milestone tracker in `README.md` as the public-facing progress summary.

Use this file as the detailed internal roadmap and milestone-definition reference.

If `README.md` and this file ever disagree, update both so milestone status, stopping point, and next action stay aligned.

For P1-2, milestone numbering continues from P1-1 so the dependency remains obvious across the portfolio.

---

# Investigation Direction

After the core telemetry pipeline is validated, later work can branch into:

- authentication investigations
- Sysmon process analysis
- PowerShell activity review
- scan detection
- web attack review
- endpoint comparison across platforms
- tuning and filtering improvements

This repo should stay focused on making telemetry usable, searchable, and validated before deeper casework expands elsewhere.

---

# Supporting Host Map

## Already Available from P1-1

| VM Name | Role |
|---|---|
| FW-EDGE01 | pfSense firewall/router |
| AD-DC01 | Domain Controller / possible collector candidate |
| TEST-WIN10-LAN1 | First Windows endpoint / future AD-WIN10 |
| TEST-WIN10-LAN2 | LAN2 Windows endpoint |
| ATTACK-KALI01 | Kali traffic generator |
| VULN-METASPLOITABLE2 | Vulnerable Linux target |
| SIEM-SPLUNK01 | Splunk destination |

## Added During P1-2 as Needed

| VM Name | Planned Milestone | Role |
|---|---:|---|
| LAN1-FILE01 | Milestone 10 | File server / SMB telemetry source |
| AD-WIN11 | Milestone 11 | Second Windows endpoint |
| VULN-DVWA01 | Milestone 12 | Vulnerable web telemetry source |
| TEST-WIN2019-LAN2 | Milestone 13 | Windows Server telemetry source |
| SCAN-OPENVAS01 | Milestone 14 | Vulnerability scan telemetry source |
| VULN-WEBGOAT01 | Milestone 15 | Optional vulnerable web telemetry source |
| VULN-UBU-OLD | Milestone 16 | Optional outdated Linux telemetry source |

---

# Build Priority If Time or Hardware Gets Tight

Build in this order:

1. LAN1-FILE01
2. AD-WIN11
3. VULN-DVWA01
4. TEST-WIN2019-LAN2
5. SCAN-OPENVAS01
6. VULN-WEBGOAT01
7. VULN-UBU-OLD

Reason:

- This order expands telemetry coverage and portfolio value first.

---

# Diagram Status Labels

The network diagram can include current, planned, optional, and future systems, but each system must have a clear status label.

Use these labels:

- Current
- Planned
- Optional
- Future

Examples:

| System | Diagram Label |
|---|---|
| `LAN1-FILE01` | Planned Milestone 10 |
| `VULN-DVWA01` | Planned Milestone 12 |
| `VULN-WEBGOAT01` | Optional Milestone 15 |

Rule:

- The diagram can be ambitious, but it must stay honest.
- Do not imply a VM is already built if it is only planned.

---

# Milestone Update Rules

When a milestone is completed, update:

- `README.md`
- `docs/current-status.md`
- `docs/milestone-schedule.md`
- `docs/build-notes.md`
- any relevant collector, WEF, Sysmon, Wazuh, Elastic, Splunk, endpoint, or validation documentation
- screenshots under `screenshots/`

Do not mark a milestone complete unless the completion criteria are met and validation evidence exists.

A milestone is not complete just because software was installed. It is complete when the intended telemetry function works and evidence has been documented.

Examples:

- Milestone 7 is not complete just because Sysmon was installed.
- Milestone 7 is complete when Sysmon is generating local events and evidence is documented.
- Milestone 8 is not complete just because a subscription exists.
- Milestone 8 is complete when events are arriving at the collector and documented.
- Milestone 9 is not complete just because a destination was configured.
- Milestone 9 is complete when telemetry is searchable in the destination and documented.

---

# Documentation Expectations Per Milestone

Each milestone should produce or update at least one GitHub artifact.

Common documentation files to update:

- `README.md`
- `docs/current-status.md`
- `docs/milestone-schedule.md`
- `docs/build-notes.md`

Milestone-specific artifacts should use this naming pattern:

```text
docs/milestone-XX-topic-name.md
```

Examples:

```text
docs/milestone-07-sysmon-deployment.md
docs/milestone-08-wef-subscription-setup.md
docs/milestone-09-splunk-ingestion-validation.md
docs/milestone-12-dvwa-telemetry-validation.md
docs/milestone-14-scan-telemetry-review.md
```

---

# Validation Standard

Every major telemetry task should include proof.

Good validation examples:

- Sysmon event visible locally
- WEF event visible at collector
- Wazuh event visible in dashboard or search
- Elastic event visible in search
- Splunk search result
- Screenshot
- Short explanation of what the result proves

Bad validation examples:

- "It worked"
- "Configured"
- "Done"
- "Installed"
