# PROJECT OVERVIEW

In this lab, I'll analyze real Windows Security logs to detect unauthorized access attempts. This is exactly the daily work of a Level 1 SOC Analyst: reviewing security events, identifying suspicious patterns, and documenting findings.

**Real-world scenario:** I'm a SOC Analyst at a company. The alert system has detected multiple 4625 events (Failed Logon) on a critical server. My job is to investigate whether this is:

   - A brute force attack in progress
   - A legitimate user with an incorrect password
   - Normal system activity

## TOOLS USED

| **Tool**              | **Purpose**                 | **Why This Tool?**                                      |
|-----------------------|-----------------------------|----------------------------------------------------------|
| PowerShell            | Log analysis and filtering  | Native to Windows, commonly used in enterprise SOCs      |
| Windows Event Viewer  | Visual log inspection       | Standard GUI for quick analysis                          |
| Excel/CSV             | Data correlation            | Pattern analysis across large data volumes               |
| MITRE ATT&CK          | Threat mapping              | Industry-standard framework                              |


## OBJECTIVE

   - Extract and analyze Windows Security Event Logs
   - Identify events 4625 (Failed Logon)
   - Detect brute force attack patterns
   - Correlate events by user, IP, and time
   - Document findings in professional SOC format
   - Map to MITRE ATT&CK Technique T1110 (Brute Force)

## LAB ENVIRONMENT

   - System: Windows 10/11 or Windows Server 
   - PowerShell: Version 5.1+ 
   - Tools: Event Viewer 
   - Sample Data: Security.evtx 
   - Analysis Location: 04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/


## PREREQUISITES

   - Lab 1.1.1 completed
   - PowerShell installed 
   - Administrator permissions 
   - 2-3 hours of dedicated time
   - Investigative mindset

## STEP BY STEP - LABORATORY 1.1.2

**STEP 1.1.2.1: Verify PowerShell and Permissions**

1. Open PowerShell as Administrator Windows 10/11:

- Press Windows + X
- Select “Windows PowerShell (Admin)” or “Terminal (Admin)”
- Run the command:

```powershell
Get-WinEvent -LogName Security -MaxEvents 1
```

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-1.png)

**STEP 1.1.2.2: Download Sample Security Logs**

I used sample logs for this lab and Using public datasets

- ***Create a sample CSV file with realistic data***

```powershell
$sampleData = @"
TimeCreated,Id,Level,Message,TargetUserName,IpAddress,WorkstationName
2025-01-17 08:15:23,4625,4,An account failed to log on,Administrator,192.168.1.100,DESKTOP-ATTACKER
2025-01-17 08:15:35,4625,4,An account failed to log on,Administrator,192.168.1.100,DESKTOP-ATTACKER
```

- ***Save to CSV file***

```powershell
$sampleData | Out-File -FilePath “Security-Sample-Analysis.csv” -Encoding UTF8

Write-Host “Sample data created: Security-Sample-Analysis.csv” -ForegroundColor Green
```

- ***Run the script and verify***

```powershell
Get-Content “Security-Sample-Analysis.csv”
```

- ***Import and view as table***

```powershell
Import-Csv “Security-Sample-Analysis.csv” | Format-Table
```

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-3.png)

## STEP 1.1.2.3: Basic Analysis - Identify Events 4625

What is Event ID 4625? Event ID 4625: “An account failed to log on” causes...User forgot their password, Account locked due to policy,  when entering password

Suspicious causes could be Brute force attack, Credential stuffing, Password spraying


**Analyze with PowerShell**

1. Import the sample CSV file.

```powershell
$logs = Import-Csv “Security-Sample-Analysis.csv”
```

2. View all logs.

```powershell
$logs | Format-Table -AutoSize
```

3. Filter only events 4625 (Failed Logon).

```powershell
$failedLogins = $logs | Where-Object {$_.Id -eq 4625}
```

4. Count how many there are

```powershell
$failedLogins.Count
```

5. View details

```powershell
$failedLogins | Format-Table TimeCreated, TargetUserName, IpAddress, WorkstationName -AutoSize
```

**Why use ```Where-Object``` instead of filtering manually?**

***Scalability*** Works with 10 or 10,000 events

***Reproducibility*** The command is documentable

***Automation*** Can be converted into a script for continuous monitoring


![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-4.png)

## STEP 1.1.2.4: Advanced Analysis - Detecting Attack Patterns

**Pattern 1: Multiple Failed Attempts from Same IP**

- Group failed attempts by IP address

```powershell
$failedLogins | Group-Object IpAddress | 
    Select-Object Name, Count | 
    Sort-Object Count -Descending
```

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-5.png)


***Analysis***

   - 192.168.1.100: 5 failed attempts = Possible brute force attack
   - 10.0.0.50: 2 attempts = Requires investigation
   - 192.168.1.50: 1 attempt = Probably legitimate user with typo

**Pattern 2: Time-Based Analysis (Velocity Check)**

***Convert TimeCreated to DateTime and analyze speed***

```powershell
$failedLogins | ForEach-Object {
    $_ | Add-Member -NotePropertyName "DateTime" -NotePropertyValue ([DateTime]$_.TimeCreated) -Force
}
```

***Sort by time***

```powershell
$failedLogins | Sort-Object DateTime | 
    Select-Object DateTime, TargetUserName, IpAddress
```

***Calculate time between attempts***

```powershell
$ip100Attempts = $failedLogins | Where-Object {$_.IpAddress -eq “192.168.1.100”} | Sort-Object DateTime


Write-Host “`n Analyzing 192.168.1.100 attempts:” -ForegroundColor Cyan
for ($i = 1; $i -lt $ip100Attempts.Count; $i++) {
    $timeDiff = ([DateTime]$ip100Attempts[$i].DateTime) - ([DateTime]$ip100Attempts[$i-1].DateTime)
    Write-Host “  Attempt $($i+1): $($timeDiff.TotalSeconds) seconds after previous” -ForegroundColor Yellow
}
```

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-6.png)


- Brute Force Indicators:

  - **Normal** 1-2 failed attempts hours apart
  - **Suspicious** 3-5 attempts within minutes
  - **Confirmed attack** 5+ attempts within seconds/minutes

In my sample:

5 attempts in ~3 minutes → Probable brute force attack

**Pattern 3: Targeted Accounts Analysis**

***Which accounts are being targeted?***

```powershell
$failedLogins | Group-Object TargetUserName | 
    Select-Object Name, Count | 
    Sort-Object Count -Descending
```

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-7.png)



- **Why is this critical?**

  - **Administrator** = Has maximum privileges
  - **dbadmin** = Access to sensitive databases
  - **Attacking privileged accounts** = High Severity Incident

**Pattern 4: Success After Multiple Failures**

Search for successful logins (4624) after failed ones (4625)


```powershell
$successfulLogins = $logs | Where-Object {$_.Id -eq 4624}

Write-Host "`n Checking for successful logins after failures:" -ForegroundColor Cyan

foreach ($success in $successfulLogins) {
    $priorFailures = $failedLogins | Where-Object {
        $_.TargetUserName -eq $success.TargetUserName -and
        $_.IpAddress -eq $success.IpAddress -and
        ([DateTime]$_.TimeCreated) -lt ([DateTime]$success.TimeCreated)
    }
    
    if ($priorFailures.Count -gt 0) {
        Write-Host " ALERT: User '$($success.TargetUserName)' from $($success.IpAddress) succeeded after $($priorFailures.Count) failures" -ForegroundColor Red
        Write-Host "  - Failed attempts: $(($priorFailures | Select-Object -First 1).TimeCreated) to $(($priorFailures | Select-Object -Last 1).TimeCreated)"
        Write-Host "  - Successful login: $($success.TimeCreated)"
        Write-Host "  -  SEVERITY: HIGH - Possible compromised credentials"
    }
}
```

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-8.png)


- **Critical finding in my sample**

***Administrator from 192.168.1.100***

  - 5 failed attempts (08:15-08:16)
  - 1 successful login (09:00)
**Conclusion:** Credentials likely compromised after brute force attack.

## STEP 1.1.2.5: Correlation and Threat Intelligence

**GeoIP Analysis (Simulated)**

In a real SOC, I'd GeoIP API and Here I simulate it with IP range analysis.

```powershell
function Analyze-IpAddress {
    param($ip)
    
    if ($ip -like "192.168.*") {
        return "Internal Network (RFC1918)"
    } elseif ($ip -like "10.*") {
        return "Internal Network (RFC1918)"
    } else {
        return "External/Unknown"
    }
}

Write-Host "`n IP Address Analysis:" -ForegroundColor Cyan
$failedLogins | Select-Object IpAddress -Unique | ForEach-Object {
    $analysis = Analyze-IpAddress -ip $_.IpAddress
    Write-Host "  $($_.IpAddress): $analysis"
}
```

- **Analysis**

   - 192.168.1.100: Internal network → Possible compromised internal device
   - 10.0.0.50: Internal network → Server or device in DMZ
   - If external IP → Review firewall and VPN logs

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-9.png)

## MITRE ATT&CK Mapping

```powershell
Write-Host "` MITRE ATT&CK Mapping:" -ForegroundColor Magenta
Write-Host "════════════════════════════════════════════"
Write-Host "Technique: T1110 - Brute Force"
Write-Host "Sub-Technique: T1110.001 - Password Guessing"
Write-Host "Tactic: Credential Access"
Write-Host "Detection: Event ID 4625 (Multiple occurrences)"
Write-Host "Evidence: 5 failed attempts in 3-minute window"
Write-Host "════════════════════════════════════════════"
```
![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-10.png)

***Create a report field***

```powershell
@"
===========================================
MITRE ATT&CK Mapping:
===========================================
Technique: T1110 - Brute Force
Sub-Technique: T1110.001 - Password Guessing
Tactic: Credential Access
Detection: Event ID 4625 (Multiple occurrences)
Evidence: 5 failed attempts in 3-minute window
===========================================
"@ | Out-File -FilePath "MITRE-Attack-Report.txt"
```

![Lab-1.1.2-Windows-Event-Analysis1](/assets/screenshots/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/Lab-1.1.2-Windows-Event-Analysis-11.png)

**Why map to MITRE ATT&CK?**

**Common language:** Global SOCs use this framework

**Documentation:** Facilitates reporting to management

**Threat Intelligence:** Allows correlation with known campaigns

