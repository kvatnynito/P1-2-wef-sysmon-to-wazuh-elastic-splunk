# Elastic Ingestion
Status: Draft (to be updated during implementation)

## Goals
- Ingest forwarded Windows/Sysmon (a free Microsoft tool that records detailed system activity like process launches and network connections) logs into Elastic (the Elasticsearch and Kibana stack — a search and visualization platform used to store and query log data)
- Confirm searchable fields and basic dashboards

## Implementation notes to add
- Ingestion (the process of receiving log data into a platform like Splunk) method (Winlogbeat/Elastic Agent/Filebeat/etc.)
- Index (the storage bucket Splunk uses to organize incoming log data) naming and mapping notes
- Any parsing/field fixes performed

## Evidence to add
- Screenshot: Kibana (the visualization front-end for Elasticsearch, used to search and view log data in a browser) Discover query showing Sysmon/Windows events
- Saved search or KQL used
- Notes on field normalization (if needed)
