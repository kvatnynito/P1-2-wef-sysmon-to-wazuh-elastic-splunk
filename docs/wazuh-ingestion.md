# Wazuh Ingestion
Status: Draft (to be updated during implementation)

## Goals
- Ingest (the process of receiving log data into a platform like Splunk) forwarded Windows events and/or Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) into Wazuh (an open-source security platform that collects agent data, generates alerts, and supports endpoint monitoring)
- Validate visibility and alerting

## Implementation notes to add
- Wazuh components used (agent/manager/indexer)
- Ingestion method (agent on collector vs endpoint)
- Any rules/decoders tuned

## Evidence to add
- Screenshot: Wazuh agent connected
- Screenshot: Event/alert visible in Wazuh dashboard
- Example alert (sanitized) and what triggered it
