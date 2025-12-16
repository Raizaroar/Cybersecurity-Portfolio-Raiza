# Suspicious Traffic Analysis with Multi-System Correlation

## Project Overview
This project analyzes anomalous network traffic detected by Splunk, correlates events with existing tickets in ServiceNow, and generates a comprehensive incident report that demonstrates forensic analysis and data correlation skills.

-**Duration**: 

2 hours
Difficulty: Entry Level
Platform: Kali Linux



**Tools Used**

- **Kali Linux**: Network traffic generation
- **Splunk**: Traffic analysis and anomaly detection
- **ServiceNow**: Incident management system
- **tcpdump**: Packet capture
- **Wireshark**: Packet analysis (optional)

**Objective**

Detect and analyze suspicious network traffic, correlate events between Splunk and ServiceNow tickets, and document the complete incident with remediation recommendations.

 **Lab Environment**

- Kali Linux VM
- Splunk (your current subscription)
- ServiceNow Developer Instance
- Local network connection

**Prerequisites**

- tcpdump installed (sudo apt install tcpdump)
- sudo permissions on Kali Linux
- ServiceNow account configured
- Basic networking knowledge

## PHASE 1: PREPARING THE ENVIRONMENT

   - **Step 1.1: Check the system**


```which tcpdump``` Verify that tcpdump is installed.

   - **Step 1.2**

```ip a``` Verify the network interface

> [!NOTE] 
> Write down the name of your interface; we will use it in the next step.

   - **Step 1.3: Create laboratory structure**


```bash 
cd ~/Documents
mkdir -p cybersecurity-labs/Lab02-Network-Traffic-Analysis
cd cybersecurity-labs/Lab02-Network-Traffic-Analysis
mkdir screenshots evidence queries
ls -l
```

![ServiceNowlab2](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab02-ServiceNow-Suspicious-Traffic-Analysis/Lab02-ServiceNow-Suspicious-Traffic-Analysis.png)


