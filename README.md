# Graylog SIEM Home Lab

## Overview

This project documents the deployment of a small Security Information and Event Management (SIEM) environment using Graylog and NXLog.

The project was created to gain hands-on experience with centralized logging, Windows event analysis, Linux administration, networking, log collection, and security monitoring.

The current SIEM monitors a single Windows PC, with Graylog running on a Kali Linux virtual machine.

## Architecture

```text
Windows PC
    │
    │ NXLog
    ▼
VirtualBox Network
    │
    ▼
Kali Linux VM
    │
    │ Graylog
    ▼
Log Search & Dashboards
```

## Environment

| System | Role |
|---|---|
| Windows PC | Log source |
| Kali Linux VM | Graylog SIEM server |

## Technologies

| Technology | Purpose |
|---|---|
| Graylog | SIEM and centralized log management |
| NXLog | Windows log collection and forwarding |
| Windows | Log source |
| Kali Linux | Graylog server |
| VirtualBox | Virtualization and networking |
| PowerShell | Windows administration |
| Windows Event Viewer | Event investigation |

## Goals

- Deploy a working SIEM environment
- Centralize Windows logs
- Learn Graylog
- Learn NXLog
- Investigate Windows events
- Practice log searching
- Create SIEM dashboards
- Develop troubleshooting skills

## Skills Demonstrated

- SIEM deployment
- Centralized logging
- Windows event analysis
- Log collection
- Log forwarding
- Graylog
- NXLog
- Linux administration
- Windows administration
- Network troubleshooting
- Log searching
- Dashboard creation
- Technical problem solving

## Environment

The current environment contains one Windows log source and one Kali Linux VM running Graylog.

The project can be expanded in the future by adding additional systems and log sources.
