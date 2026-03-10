# Wazuh SIEM Lab

This repository documents a hands-on SIEM lab built using Wazuh.  
The goal of this project is to demonstrate practical SOC skills including SIEM deployment, agent onboarding, log ingestion, detection, and visualization.

## Scenario
This lab simulates a small enterprise environment where a SOC team monitors Windows and Linux endpoints using Wazuh SIEM. The objective is to detect unauthorized access, suspicious PowerShell activity, and file integrity violations.

## Lab Environment
- Ubuntu Server (Wazuh Manager, Indexer, Dashboard)
- Windows 10 Endpoint
- Ubuntu Linux Endpoint

## 📂 Lab Progress

### Week 1 — Environment Setup  
Secure virtual lab deployment with Windows and Ubuntu VMs.  
🔗 [View Week 1 Documentation](Week-1/README.md)

### Week 2 — Wazuh SIEM Installation  
All-in-one Wazuh deployment (Manager, Indexer, Dashboard).  
🔗 [View Week 2 Documentation](Week-2/README.md)

### Week 3 — Agent Onboarding & Log Collection  
Endpoint registration, log ingestion verification, and alert visualization.  
🔗 [View Week 3 Documentation](Week-3/README.md)

### Week 4 — Baseline Monitoring & FIM Validation  
Baseline behavior analysis, Windows security event investigation, and File Integrity Monitoring (FIM) testing.  
🔗 [View Week 4 Documentation](Week-4/README.md)

### Week 5 — Sysmon & Advanced Windows Logging  
Enterprise-grade Windows telemetry with Sysmon, SwiftOnSecurity config, and PowerShell Script Block Logging feeding the SIEM.  
🔗 [View Week 5 Documentation](Week-5/README.md)
