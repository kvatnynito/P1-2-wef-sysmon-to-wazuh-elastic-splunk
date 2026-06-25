# Windows Event Forwarding (WEF) Setup
Status: Collector selected; WEF implementation not started

## Goals
- Configure a source-initiated subscription (a WEF configuration that tells the collector which endpoints to pull logs from and which events to collect)
- Forward Security + Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) logs from endpoints to the collector (a Windows machine that receives forwarded logs from multiple endpoints)

## Collector Placement

The selected WEF collector is a dedicated Windows Server VM named `WEC01`.

| Setting | Value |
|---|---|
| Collector | `WEC01` |
| IP address | `10.10.10.30` |
| Domain | `corp.local` |
| Domain DNS / DC | `AD-DC01` at `10.10.10.10` |
| Collector role | Dedicated WEF collector only |

`WEC01` was selected instead of placing the WEF collector role on `AD-DC01`. This keeps collection duties separate from domain controller duties, which better matches production SOC architecture and avoids expanding the role of the highest-value server in the domain.

## Readiness Validation

Before WEF subscription work begins, the first endpoint readiness checks were completed:

- `TEST-WIN10-LAN1` is joined to `corp.local`.
- `TEST-WIN10-LAN1` receives `AD-DC01` (`10.10.10.10`) as DNS through pfSense DHCP.
- `TEST-WIN10-LAN1` resolves `corp.local`.
- `TEST-WIN10-LAN1` resolves `WEC01.corp.local` to `10.10.10.30`.
- `TEST-WIN10-LAN1` reaches `WEC01.corp.local` on WinRM TCP `5985`.

ICMP ping to `WEC01` is blocked or unvalidated, but WinRM TCP `5985` is the WEF-relevant transport path and is reachable.

## Implementation notes to add
- Collector host (WEC (Windows Event Collector — the server-side role that receives forwarded Windows logs)): `WEC01`
- Subscription type: Source-initiated
- Events forwarded: [channels / event IDs]

## Evidence to add
- Screenshot: Subscription created
- Screenshot: Forwarded events visible on collector (Event Viewer)
- Screenshot: Endpoint successfully enrolled (if applicable)
