# Diagrams
Status: Draft (to be updated during implementation)

## TODO: pipeline-diagram.png (Checklist)
Create and upload: `pipeline-diagram.png`

Include (high-level only):
- [ ] Endpoints: AD-WIN10, AD-WIN11
- [ ] Telemetry (the stream of log and event data collected from systems to support monitoring and investigation) sources: Windows Security + Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections)
- [ ] Transport: WEF (Windows Event Forwarding — a built-in Windows feature that pushes event logs from endpoint machines to a central collector) (source-initiated subscription (a WEF configuration that tells the collector which endpoints to pull logs from and which events to collect)) → dedicated collector `WEC01` (Windows Event Collector — the server-side role that receives forwarded Windows logs)
- [ ] Downstream platforms: Wazuh (an open-source security platform that collects agent data, generates alerts, and supports endpoint monitoring), Elastic (the Elasticsearch and Kibana stack — a search and visualization platform used to store and query log data), Splunk Free
- [ ] Arrow labels showing flow direction (Endpoints → Collector → Platforms)

Do NOT include:
- [ ] WAN (Wide Area Network — the public internet-facing side of the network) / public IPs, domains/DDNS, VPN endpoints/keys
- [ ] Port forwards, exposed services, credentials/secrets

Optional:
- [ ] Upload editable source file too (e.g., `pipeline-diagram.drawio`)
