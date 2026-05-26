# Session Notes

## Purpose

This file is a short handoff note for resuming work in another assistant or tool.

It does not replace:

- `docs/current-status.md`
- `docs/milestone-schedule.md`
- the private runbook repo at `/home/kvatny/homelab/P1-2-private-runbooks`

Use `docs/current-status.md` as the source of truth for project status. Use this file to quickly understand what happened in the last working session.

## Last Session

**Date:** 2026-05-26  
**Active milestone:** Milestone 6 - Logging Foundation  
**Current objective:** Prove pfSense and Windows logs flow into Splunk.

## What Happened

- Confirmed Milestone 6 is still in progress.
- Configured Splunk to listen for pfSense syslog on UDP `5514`.
- Selected source type `syslog`.
- Used app context `search`.
- Used index `default` / `main`.
- Confirmed Splunk showed `UDP input has been created successfully`.
- Opened pfSense and navigated to `Status > System Logs`.
- Reached `Status > System Logs > Settings`.
- Confirmed the visible top section is `General Logging Options`.
- Saved Step 3 and Step 4 screenshots into the private runbook repo.

## Current Stopping Point

We stopped on the pfSense `Status > System Logs > Settings` page.

Next safe step:

```text
Scroll down to find the remote logging / remote syslog server section.
```

Do not mark pfSense logging as validated yet.

## Not Finished

- pfSense remote logging has not been configured yet.
- pfSense logs have not been validated in Splunk.
- Windows Event Log forwarding from `TEST-WIN10-LAN1` has not been configured.
- Windows logs have not been validated in Splunk.

## Repos Updated

Public repo:

```text
d287e4d Update Milestone 6 pfSense logging checkpoint
```

Private runbook repo:

```text
f8628eb Add pfSense logging settings checkpoint
```

## Resume Checklist

1. Read `docs/current-status.md`.
2. Confirm Milestone 6 is still active.
3. Confirm Splunk UDP `5514` input remains configured.
4. Continue pfSense remote logging setup from `Status > System Logs > Settings`.
5. Update the private runbook repo as screenshots are captured.
6. Do not advance to Windows forwarding until pfSense logs are searchable in Splunk.
