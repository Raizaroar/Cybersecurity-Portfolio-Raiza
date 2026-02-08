#  LAB 2.1.2: Advanced Splunk - Detection Engineering & Threat Hunting

## PROJECT OVERVIEW

Today, elevating your Splunk level from basic operator to detection engineer. I'll learn advanced SPL techniques, create complex correlations, implement Sigma rules, and conduct your first proactive threat hunting session.

**Real-World Application:**

Detection engineers create the rules that SOC analysts use. Threat hunters look for threats that automatic alerts do not detect. Today you will learn about both roles.

**Key Difference:**

1. Day 8: Reactive searches (“What happened?”)
2. Day 9: Proactive hunting (“What is hidden?”)

## LAB ENVIRONMENT

- Splunk: Running from Day 8
- Data: Existing indexes + new hunt datasets

## PREREQUISITES

- Lab 2.1.1 completed 
- Splunk running with data indexed
- Understanding of basic SPL
- MITRE ATT&CK familiarity


## STEP BY STEP - LAB 2.1.2

**STEP 2.1.2.1: Advanced SPL Techniques**

**Technique 1:** ***Regular Expressions (regex)***

Use Case: Extract specific patterns from unstructured logs

- Example: **Extract IP addresses from messages**

```spl
index="windows_security"
| rex field=Message "(?<extracted_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
| table _time, Message, extracted_ip
```

![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk1.png)

Detection Use Case: Find obfuscated PowerShell attacks

- Example: **Extract PowerShell encoded commands**

```spl
index="windows_security" EventCode=4688
| rex field=CommandLine "-enc(?:oded)?(?:command)?\s+(?<encoded_cmd>\S+)"
| table _time, User, CommandLine, encoded_cmd
```

![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk2.png)

**Technique 2:**  ***eval (Field Calculations)***

Use Case: Create new fields based on calculations

- Example: **Calculate time between events**

```spl
index="windows_security" EventCode=4625
| sort _time
| streamstats current=f last(_time) as last_time by User
| eval time_diff = _time - last_time
| where time_diff < 60
| table _time, User, ComputerName, time_diff
```

![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk3.png)

1. **What it does:** Finds failed logins less than 60 seconds apart
2. **Detection:** High-velocity brute force


- Example: **Categorize severity**

```spl
index="firewall"
| eval severity = case(
    action="DENY" AND port IN (445,3389,22), "CRITICAL",
    action="DENY" AND port<1024, "HIGH",
    action="DENY", "MEDIUM",
    true(), "LOW"
)
| stats count by severity
```


![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk4.png)

**What it does:** Dynamic severity assignment based on conditions

**Technique 3:** ***Macros (Reusable Search Fragments)***

#### Create a macro for common searches:

1. Define macro:

```
Settings > Advanced search > Search macros > New Search Macro
```

2. Macro definition:

```
Name: failed_logins
Definition: index="windows_security" EventCode=4625
```


![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk5.png)

3. Use macro in searches:

```spl
`failed_logins`
| stats count by User
```


![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk6.png)

**Benefits:**

- Consistency across searches
- Easy updates (change once, affects all)
- Cleaner SPL code

**Technique 4:** ***Advanced Subsearches***

- Example: **Find hosts with both malware AND C2 communication**

```spl
(index="windows_security" EventCode=4688 process_name="malware.exe")
OR (index="firewall" dest_ip="185.220.101.45")
```

![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk7.png)

1. **What it does:** Correlates process execution with network activity
2. **Detection:** Confirmed malware with active C2

**Technique 5:** ***Transaction (Event Grouping)***

- Example: **Group login attempts into sessions**

```spl
index="windows_security" (EventCode=4624 OR EventCode=4625)
| transaction User maxspan=5m
| where eventcount > 5
| table _time, User, eventcount, duration
```

![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk8.png)

**What it does:** Groups related events within 5-minute window
**Detection:** Brute force sessions with 5+ attempts

## STEP 2.1.2.2: Multi-Log Correlation Detections

1. **Correlation 1:** ***Phishing → Malware → C2***

Scenario: **Detect complete attack chain**

```spl
index="email_logs" attachment="*.exe"
| rename recipient as User
| join type=inner User
    [search index="windows_security" EventCode=4688
    | rename User as User]
| join type=inner ComputerName
    [search index="firewall_logs" dest_ip="185.220.101.45"
    | rename src_ip as ComputerName]
| table _time, User, ComputerName, attachment, dest_ip
```

**What it correlates:**

- Email with executable attachment
- Same user executes process
- Same computer contacts C2

**MITRE Mapping:**

- T1566.001 (Phishing)
- T1204.002 (User Execution)
- T1071.001 (C2 Communication)

2. **Correlation 2:** ***Failed Login → Successful Login → Privilege Escalation***

```spl
index="windows_security" EventCode=4625
| stats count as failed_attempts by User, ComputerName
| where failed_attempts > 3
| join User
    [search index="windows_security" EventCode=4624
    | stats latest(_time) as success_time by User, ComputerName]
| join User
    [search index="windows_security" EventCode=4732 Group="Domain Admins"
    | rename TargetUser as User]
| table User, failed_attempts, success_time, Group
```

![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk9.png)

**Attack Chain:**

- Brute force attack (3+ failures)
- Successful compromise
- Privilege escalation to admin

#### Severity: CRITICAL

3. **Correlation 3:** ***Port Scan → Exploitation → Data Exfiltration***

```spl
(index="firewall")
OR (index="web-access" status=200 uri="*sql*")
OR (index="firewall" bytes_out > 10000000)
| eval stage=case(
    index="firewall" AND action="SYN", "port_scan",
    index="web-access" AND uri="*sql*", "exploitation",
    index="firewall" AND bytes_out > 10000000, "exfiltration"
)
| stats dc(dest_port) as ports_scanned
        values(uri) as uri
        max(bytes_out) as bytes_out
        values(stage) as stages
    by src_ip
| where ports_scanned > 20 AND isnotnull(uri) AND bytes_out > 10000000
| rename src_ip as attacker_ip
| table attacker_ip, ports_scanned, uri, bytes_out, stages
```


![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk10.png)

**Full Kill Chain Detection:**

- Reconnaissance (port scan)
- Exploitation (SQL injection)
- Exfiltration (large upload)

## STEP 2.1.2.3: Sigma Rules Integration

***What are Sigma Rules?***

Sigma = Universal detection format

```yaml
title: Suspicious PowerShell Encoded Command
status: experimental
description: Detects suspicious encoded PowerShell commands
references:
    - https://attack.mitre.org/techniques/T1059/001/
author: Raiza
date: 2025/01/29
logsource:
    product: windows
    service: security
detection:
    selection:
        EventID: 4688
        CommandLine|contains:
            - '-enc'
            - '-encodedcommand'
            - 'frombase64string'
    condition: selection
falsepositives:
    - Legitimate admin scripts
level: high
tags:
    - attack.execution
    - attack.t1059.001
```

![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk11.png)

1. Create Your Own Sigma Rule

Example:***Detect Credential Dumping***

Create: sigma-rules/credential-dumping.yml

```yaml
title: Credential Dumping with Mimikatz
status: stable
description: Detects use of Mimikatz or similar credential dumping tools
author: Raiza
date: 2025/01/29
logsource:
    product: windows
    service: security
detection:
    selection:
        EventID: 4688
        CommandLine|contains:
            - 'mimikatz'
            - 'sekurlsa::logonpasswords'
            - 'lsadump::sam'
            - 'procdump -ma lsass'
    condition: selection
falsepositives:
    - Legitimate security testing
level: critical
tags:
    - attack.credential_access
    - attack.t1003.001
```


![AdvancedSplunkLab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk12.png)

## STEP 2.1.2.4: Build Threat Hunting Dashboard

Dashboard Name: "Active Threat Hunting"

1. Panel: Process Execution Heatmap

```spl
index="windows_security" EventCode=4688
| timechart count by NewProcessName limit=10
```

- Visualization: Line chart
- Purpose: Identify process execution spikes

2. Panel: Rare Process Execution

```spl
index="windows_security" EventCode=4688
| stats count by NewProcessName
| where count < 5
| sort count
```

- Visualization: Table
- Purpose: Highlight anomalies for investigation

3. Panel: Off-Hours Authentication

```spl
index="windows_security" EventCode=4624
| eval hour=strftime(_time, "%H")
| where hour < 6 OR hour > 22
| stats count by User, hour
```

- Visualization: Bar chart
- Purpose: Identify after-hours activity

4. Panel: LOLBin Abuse Detection

```spl
index="windows_security" EventCode=4688
| search NewProcessName IN ("*certutil.exe", "*bitsadmin.exe", "*rundll32.exe")
| timechart count by NewProcessName
```

- Visualization: Area chart
- Purpose: Monitor living-off-the-land techniques

5. Panel: Network Anomaly Score

```spl
index="firewall_logs"
| stats dc(dest_ip) as unique_destinations, sum(bytes_out) as total_upload by src_ip
| eval anomaly_score = (unique_destinations * 0.3) + (total_upload / 1000000)
| where anomaly_score > 10
| sort -anomaly_score
```

- Visualization: Table with color formatting
- Purpose: Prioritize hosts for investigation

[See Dashboard with Report Click Here](https://github.com/Raizaroar/Cybersecurity-Portfolio-Raiza/blob/main/labs/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Threat-Hunt-Report.md) 


## Conclusion 

The project consolidated the ability to transform generic rules into actionable detections within Splunk, strengthening the practice of Threat Hunting and demonstrating that the combination of Sigma + Splunk is an effective way to increase security maturity in any organization.

