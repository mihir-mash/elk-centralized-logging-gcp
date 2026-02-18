# Centralized Logging & Monitoring using ELK Stack on GCP

This project demonstrates a cloud-based centralized logging architecture using the ELK stack (Elasticsearch, Logstash, Kibana) deployed on Google Cloud Platform. 

Two virtual machines were provisioned:
- monitor-vm → Hosts Elasticsearch, Logstash, and Kibana
- web-vm → Hosts Nginx and Filebeat for log generation and shipping

The system collects web server logs in real time, parses them using Logstash, indexes them in Elasticsearch, and visualizes them through Kibana dashboards.

---

## Architecture Overview

Log Flow:

Nginx (web-vm)  
→ Filebeat  
→ Logstash (monitor-vm)  
→ Elasticsearch  
→ Kibana Dashboard

This setup enables centralized logging, real-time monitoring, and structured log analysis in a cloud environment.

---

## Infrastructure Setup (GCP)

Two Ubuntu 22.04 virtual machines were created:

### monitor-vm
- Machine Type: e2-medium
- Services: Elasticsearch, Logstash, Kibana

### web-vm
- Machine Type: e2-small
- Services: Nginx, Filebeat

Firewall ports enabled:
- 22 (SSH)
- 5044 (Beats input)
- 9200 (Elasticsearch)
- 5601 (Kibana)

---

## ELK Stack Installation (monitor-vm)

### Elasticsearch
- Installed using official Elastic repository
- Security disabled for lab testing
- Verified using:
curl http://localhost:9200/


### Logstash
Configured pipeline:

- Beats input on port 5044
- Grok filter for parsing Nginx access logs
- Output to Elasticsearch with time-based index:
shopez-logs-YYYY.MM.dd


### Kibana
- Connected to Elasticsearch
- Created index pattern: `shopez-logs-*`
- Built dashboards for traffic and error visualization

---

## Log Collection (web-vm)

### Nginx
Installed and configured to generate access logs at:
/var/log/nginx/access.log


### Filebeat
Configured to monitor Nginx logs and forward them to Logstash on monitor-vm.

Configuration verified using:
filebeat test config


---

## Traffic Simulation

A Bash script was created to simulate realistic web traffic including:

- GET
- POST
- PUT
- DELETE

Requests were generated against multiple endpoints to produce various HTTP status codes (200, 404, 405).

This created dynamic log entries that were automatically shipped and visualized.

---

## Log Verification

Elasticsearch indices verified using:

curl -X GET http://localhost:9200/_cat/indices?v


Increasing document count confirmed successful log ingestion.

---

## Kibana Visualization

Kibana dashboards were configured to display:

- HTTP status code distribution
- Request method breakdown
- Traffic trends over time
- Client IP analysis

This enables real-time monitoring and troubleshooting capabilities.

---

## Key DevOps Concepts Demonstrated

- Centralized log aggregation
- Cloud-based infrastructure deployment (GCP)
- Log parsing using Grok filters
- Time-based indexing strategy
- Real-time observability
- Distributed system monitoring

---

## Technologies Used

- Google Cloud Platform (Compute Engine)
- Elasticsearch
- Logstash
- Kibana
- Filebeat
- Nginx
- Ubuntu 22.04
- Bash

---

## Outcome

Successfully implemented a production-style centralized logging pipeline on cloud infrastructure, enabling structured log analysis, monitoring, and real-time visualization using ELK stack.
