# LAB 2.1.1: SPLUNK SIEM - INSTALLATION & BASICS


## PROJECT OVERVIEW

Install and configure Splunk Free, the industry-leading SIEM platform. Index sample security logs (Windows, network, web) and learn fundamental SPL queries to search, filter, and correlate security events.

**Real-World Relevance:**

This is Day 1 of every SOC Analyst job. You'll receive logins to the SIEM and need to start hunting for threats immediately. This lab gives you that foundation.



## LAB ENVIRONMENT

Recommended Setup:

Primary: Linux VM Kali Linux
Resources: 4GB RAM minimum, 10GB disk space


## PREREQUISITES

- Week 1 completed (7/7 labs)
- 10GB free disk space
- Administrator privileges
- Internet connection 
- Sample data from Week 1 


## STEP-BY-STEP - LAB 2.1.1

**STEP 2.1.1.1: Download and Install Splunk**

DONE

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk.png)


## STEP 2.1.1.3: Create Sample Data for Indexing


1. Dataset 1: Windows Event Logs (from Lab 1.1.2)
2. Dataset 2: Web Server Logs (from Lab 1.1.6)
3. Dataset 3: Network Firewall Logs

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk1.png)



## STEP 2.1.1.4: Index Data into Splunk

***Method 1: Upload CSV (Windows Events)***

1. In Splunk Web:

Settings > Add Data > Upload


2. Select File:

Click "Select File"
Browse to: sample-data/windows-events.csv
Click "Next"

3. Set Source Type:

Source type: csv
Click "Next"


4. Input Settings

Host: localhost (or "windows-server")
Index: Create new index → "windows_security"
Click "Review"


5. Submit:

Click "Submit"
Wait for "Success" message

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk2.png)


## STEP 2.1.1.6: SPL Basics - Your First Searches

**SPL = Search Processing Language**

**Structure:**

```spl
search_command - transformation_command - formatting_command
```

1. Search: Basic Search

```spl
index="windows_security"
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk3.png)

-**What it does:** Shows all events in windows_security index

-**Use case:** Verify data ingestion

2. Search: Filter by Field

```spl
index="windows_security" EventCode=4625
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk4.png)

-**What it does:** Shows only failed login events (4625)

-**Use case:** Hunt for brute force attacks

3. Search: Time Range

```spl
index="windows_security" EventCode=4625
| where _time >= relative_time(now(), "-1h")
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk5.png)

-**What it does:** Failed logins in last hour

-**Use case:** Recent activity monitoring

4. Search: Field Extraction

```spl
index="web_logs" status=200
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk6.png)


- **What it does:** All successful HTTP requests

- **Use case:** Identify successful exploitation attempts

5. Search: Statistical Analysis

```spl
index="windows_security" EventCode=4625
| stats count by User
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk7.png)

- **What it does:** Count failed logins per user

- **Use case:** Identify targeted accounts


6. Search: Detect SQL Injection

```sql
index="web_logs" 
| search "OR 1=1" OR "UNION SELECT" OR "' OR '"
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk8.png)

- **What it does:** Find SQL injection attempts in web logs

-**Use case:** Detect web application attacks

7. Search : Time Chart (Visualization)

```spl
index="firewall" DENY
| timechart count by src_ip
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk9.png)


- **What it does:**  Graph denied connections over time by source IP

- **Use case:**  Identify attack patterns, spikes in malicious traffic


8. Search: Detect C2 Beaconing

```sql
index="firewall_logs" 185.220.101.45"
| stats count by _time 
```

![Lab2.1.1-Splunk](/assets/screenshots/04-SIEM-Projects/Lab2.1.1-Splunk-Basics/Lab2.1.1-Splunk10.png)


- **What it does:** Correlate process creation with network connections

- **Use case:**  Link malware execution to network activity

10. Search : Detect Brute Force

```spl
index="windows_security" EventCode=4625
| stats count by User, ComputerName
| where count > 3
```

- **What it does:** Find users with 3+ failed logins  

- **Use case:** Automated brute force detection