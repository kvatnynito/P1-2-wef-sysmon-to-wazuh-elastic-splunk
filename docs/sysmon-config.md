# Sysmon Configuration
Status: Milestone 8 active - planned / not validated

## Goals
- Install Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) on endpoints
- Use a tuned config to capture high-value events
- Validate local Sysmon event generation on `TEST-WIN10-LAN1` before configuring WEF

## Milestone 8 Scope

Milestone 8 is local validation only. The first proof point is the `Microsoft-Windows-Sysmon/Operational` log on `TEST-WIN10-LAN1`. WEF forwarding, collector-side validation, and downstream ingestion belong to later milestones.

## Implementation notes to add during validation
- Sysmon version used
- Config source (e.g., SwiftOnSecurity / custom)
- Config file name and location
- Key Event IDs enabled (e.g., 1, 3, 7, 10, 11, 13, 22)
- Test activity used to generate local events

## Planned local validation checks
- Confirm Sysmon service is running
- Confirm `Microsoft-Windows-Sysmon/Operational` exists in Event Viewer
- Generate controlled local test activity
- Confirm recent Sysmon events are visible locally
- Record at least one Event ID and what it proves

## Evidence to add
- Screenshot: Sysmon service running
- Screenshot: Example Sysmon event visible locally on endpoint
- Config file reference (sanitized): /configs/sysmonconfig.xml
