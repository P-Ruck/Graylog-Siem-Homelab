# Graylog SIEM Home Lab — Troubleshooting Log

This document records troubleshooting performed while building and configuring my Graylog SIEM home lab.

The current SIEM environment consists of:

- Windows PC — log source
- Kali Linux virtual machine — Graylog server
- NXLog — Windows log collector and forwarder
- VirtualBox — virtualization and networking

---

# 1. Windows-to-Kali Connectivity

## Problem

The Windows PC and Kali Linux VM needed to communicate before Windows logs could be forwarded to Graylog.

Initial connectivity problems prevented reliable communication between the systems.

## Investigation

Windows networking was examined using:

```powershell
ipconfig
```

IPv4 addresses were examined using:

```powershell
Get-NetIPAddress -AddressFamily IPv4 |
    Format-Table InterfaceAlias,IPAddress
```

On Kali Linux:

```bash
ip addr
```

The routing table was checked using:

```bash
ip route
```

Connectivity was tested using `ping`.

At one point, Kali Linux returned:

```text
Network is unreachable
```

This indicated that the issue needed to be investigated at the network layer before troubleshooting Graylog itself.

## Resolution

The VirtualBox networking configuration was reviewed and adjusted so the Windows PC and Kali Linux VM could communicate through the intended virtual network.

## Lesson Learned

A SIEM depends on reliable network communication.

Before troubleshooting the SIEM application, verify:

1. Network interfaces
2. IP configuration
3. Routing
4. Firewall rules
5. Connectivity

---

# 2. NXLog Log Forwarding

## Problem

NXLog was configured on Windows to collect Windows event logs and forward them to Graylog running on Kali Linux.

When logs were not appearing as expected, each component of the logging pipeline had to be investigated.

## Logging Pipeline

```text
Windows Event Logs
        ↓
      NXLog
        ↓
VirtualBox Network
        ↓
   Kali Linux
        ↓
     Graylog
        ↓
 Log Search / Dashboard
```

## Investigation

The following components were checked:

1. Windows event generation
2. NXLog service
3. NXLog configuration
4. Windows network configuration
5. VirtualBox networking
6. Windows firewall
7. Graylog input
8. Graylog messages

## Lesson Learned

A log forwarding system contains multiple independent components.

When logs are missing, each stage should be tested rather than assuming Graylog is the source of the problem.

---

# 3. Graylog Log Ingestion

## Problem

Graylog needed to be verified as receiving the logs forwarded by NXLog.

## Investigation

The Graylog input and incoming messages were examined.

The investigation involved verifying:

- Graylog input configuration
- Network connectivity
- NXLog configuration
- Windows event generation
- Incoming Graylog messages

## Troubleshooting Process

```text
Windows Event
      ↓
    NXLog
      ↓
   Network
      ↓
Graylog Input
      ↓
Graylog Message
      ↓
   Search
```

## Lesson Learned

Successful NXLog configuration does not necessarily mean that Graylog is receiving or correctly processing the logs.

Each stage of the pipeline needs to be verified independently.

---

# 4. Investigating Windows Events

## Problem

After getting Windows logs into Graylog, I needed to determine which events were useful for monitoring and investigation.

Windows generates a large number of events, making filtering and investigation important.

## Investigation

Windows Event Viewer was used to examine individual events and investigate their details.

Event IDs were used to identify specific types of activity.

One event encountered during the investigation was:

```text
Event ID 5379
```

An ActivityID was also encountered while examining Windows events.

## Information Investigated

Windows event records can contain information such as:

- Event ID
- Provider
- Timestamp
- User
- Computer
- Activity ID
- Event message
- Additional event data

## Lesson Learned

Understanding the underlying Windows event is important before creating searches or dashboards in a SIEM.

Event IDs provide a useful starting point for investigating specific types of activity.

---

# 5. Authentication Event Searching

## Problem

I initially had difficulty getting searches for successful and failed login activity to work correctly in Graylog.

## Investigation

I investigated the Windows event data being received by Graylog and examined the fields available within the messages.

Rather than assuming that a particular field existed, I learned to inspect the actual message structure first.

## Troubleshooting Process

1. Find the relevant Windows event.
2. Examine the event in Windows Event Viewer.
3. Find the corresponding message in Graylog.
4. Examine the available fields.
5. Identify useful fields for searching.
6. Create searches based on the actual collected data.

## Lesson Learned

SIEM searches should be based on the fields actually present in the collected logs.

Understanding the structure of incoming logs is important when creating reliable searches and filters.

---

# 6. Graylog Dashboard

## Problem

A Graylog dashboard was created to visualize information collected from the Windows system.

Some dashboard widgets were initially unclear, including the "Top Sources" visualization.

## Investigation

I examined what information the dashboard widgets represented and how they applied to my environment.

Because the SIEM currently monitors only one Windows system, a visualization comparing multiple log sources has limited usefulness.

## Lesson Learned

A dashboard should be designed around the questions the SIEM is intended to answer.

For a single-source environment, useful dashboard information can include:

- Event types
- Security events
- Authentication activity
- Error events
- System activity
- Event frequency

As additional systems are added, source-based visualizations can become more useful.

---

# 7. General Troubleshooting Methodology

The troubleshooting process developed into a layered approach:

```text
Windows Event Generation
          ↓
        NXLog
          ↓
       Network
          ↓
    Graylog Input
          ↓
   Message Ingestion
          ↓
      Log Fields
          ↓
       Searches
          ↓
      Dashboards
```

When a problem occurred, each layer was tested independently to identify where the failure was occurring.

This approach helped prevent problems in one part of the environment from being incorrectly attributed to another component.

---

# Skills Developed

Through troubleshooting this project, I gained hands-on experience with:

- Graylog
- NXLog
- Windows Event Logs
- Windows Event Viewer
- SIEM log ingestion
- Log searching and filtering
- Dashboard creation
- Windows security events
- Event ID investigation
- Linux administration
- Windows administration
- VirtualBox networking
- Network troubleshooting
- PowerShell
- Technical problem solving
