# Splunk Detection Use Cases

## Use Case 1: Brute Force Authentication

**Threat:** Automated password guessing attacks

**Detection Logic:**

```spl
index="windows_security" EventCode=4625
| stats count by User, ComputerName
| where count > 3
```

**MITRE ATT&CK:** T1110.001 (Password Guessing)

**Alert Threshold:** 3+ failed logins in 5 minutes

**False Positive Rate:** LOW (user typos rarely exceed 3 attempts)

**Response Actions:**
1. Lock affected account
2. Investigate source IP
3. Check for successful login after failures

---

## Use Case 2: SQL Injection Detection

**Threat:** Web application exploitation via SQL injection

**Detection Logic:**

```spl
index="web_logs"
| search "OR 1=1" OR "UNION SELECT" OR "' OR '" OR "--" OR "/*"
| stats count by src_ip, uri
```

**MITRE ATT&CK:** T1190 (Exploit Public-Facing Application)

**Alert Threshold:** Any occurrence

**False Positive Rate:** VERY LOW (legitimate traffic never contains SQL keywords)

**Response Actions:**
1. Block source IP at WAF
2. Review application logs
3. Check database for unauthorized access

---

## Use Case 3: Command & Control Beaconing

**Threat:** Malware communicating with attacker infrastructure

**Detection Logic:**

```spl
index="firewall_logs" action="ALLOW"
| stats count by src_ip, dest_ip
| where count > 10
```

**MITRE ATT&CK:** T1071.001 (Web Protocols)

**Alert Threshold:** 10+ connections to same external IP in 1 hour

**False Positive Rate:** MEDIUM (CDNs, update servers can trigger)

**Response Actions:**
1. Check dest_ip reputation (VirusTotal, AbuseIPDB)
2. Isolate affected host if malicious
3. Analyze process responsible for connections

---

## Use Case 4: Privilege Escalation

**Threat:** Attacker gaining administrative rights

**Detection Logic:**

```spl
index="windows_security" EventCode=4732
| where Group="Domain Admins" OR Group="Administrators"
| table _time, User, Group, ComputerName
```

**MITRE ATT&CK:** T1078.002 (Domain Accounts)

**Alert Threshold:** Any new member in admin groups

**False Positive Rate:** LOW (admin group changes are infrequent)

**Response Actions:**
1. Verify change was authorized
2. Review who made the change
3. Audit new admin's activities

---

## Use Case 5: Data Exfiltration

**Threat:** Large-scale data theft

**Detection Logic:**

```spl
index="firewall_logs" action="ALLOW"
| stats sum(bytes) as total_bytes by src_ip, dest_ip
| where total_bytes > 10000000
```

**MITRE ATT&CK:** T1041 (Exfiltration Over C2 Channel)

**Alert Threshold:** 10MB+ outbound to single destination

**False Positive Rate:** MEDIUM (cloud backups, updates)

**Response Actions:**
1. Identify what data was sent
2. Check if destination is corporate/approved
3. Investigate source host for compromise

---

## Use Case 6: Account Creation (Backdoor)

**Threat:** Attacker creating persistent access

**Detection Logic:**

```spl
index="windows_security" EventCode=4720
| table _time, User, NewAccount, ComputerName
```

**MITRE ATT&CK:** T1136.001 (Create Account: Local Account)

**Alert Threshold:** Any new account creation

**False Positive Rate:** LOW (account creation is logged/approved process)

**Response Actions:**
1. Verify account creation was authorized
2. Check who created account
3. Review new account's permissions

---

## Use Case 7: Suspicious Process Execution

**Threat:** Malware or hacking tools running

**Detection Logic:**

```spl
index="windows_security" EventCode=4688
| search process_name IN ("powershell.exe", "cmd.exe", "wscript.exe", "cscript.exe")
| where CommandLine CONTAINS "encoded" OR CommandLine CONTAINS "IEX" OR CommandLine CONTAINS "DownloadString"
```

**MITRE ATT&CK:** T1059.001 (PowerShell)

**Alert Threshold:** Any encoded PowerShell command

**False Positive Rate:** MEDIUM (legitimate scripts may use encoding)

**Response Actions:**
1. Decode and analyze command
2. Check parent process
3. Investigate user context

---

## Detection Coverage Matrix

| MITRE Tactic | Detections | Coverage |
|--------------|-----------|----------|
| Initial Access | SQL Injection | done |
| Execution | Suspicious Process | done |
| Persistence | Account Creation | done |
| Privilege Escalation | Admin Group Changes | done |
| Credential Access | Brute Force | done |
| Discovery | - | fail |
| Lateral Movement | - | fail |
| Collection | - | fail |
| C2 | Beaconing | done |
| Exfiltration | Large Uploads | done |

**Coverage:** 6/10 MITRE Tactics (60%)