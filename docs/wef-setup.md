# Windows Event Forwarding (WEF) Setup
Status: Draft (to be updated during implementation)

## Goals
- Configure a source-initiated subscription (a WEF configuration that tells the collector which endpoints to pull logs from and which events to collect)
- Forward Security + Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) logs from endpoints to the collector (a Windows machine that receives forwarded logs from multiple endpoints)

## Implementation notes to add
- Collector host (WEC (Windows Event Collector — the server-side role that receives forwarded Windows logs)): [name]
- Subscription type: Source-initiated
- Events forwarded: [channels / event IDs]

## Evidence to add
- Screenshot: Subscription created
- Screenshot: Forwarded events visible on collector (Event Viewer)
- Screenshot: Endpoint successfully enrolled (if applicable)
