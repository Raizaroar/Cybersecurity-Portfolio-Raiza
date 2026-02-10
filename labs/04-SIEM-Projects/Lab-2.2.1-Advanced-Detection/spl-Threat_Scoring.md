## STEP 2.2.1.7: Threat Scoring System

**Create multi-factor threat scoring.**

```spl
index="windows_security" OR index="firewall_logs" OR index="web_logs"

// Initialize score
| eval threat_score = 0

// Score factors
| eval threat_score = threat_score + 
    // Failed logins
    if(EventCode==4625, 10, 0) +
    
    // After-hours activity
    if(date_hour < 6 OR date_hour > 22, 15, 0) +
    
    // Connections to known bad IPs
    if(dest_ip IN ("185.220.101.45", "203.0.113.50"), 50, 0) +
    
    // Suspicious processes
    if(match(NewProcessName, "(?i)(mimikatz|procdump|psexec)"), 40, 0) +
    
    // Web attacks
    if(match(uri, "(?i)(union.*select|\.\.\/|<script>)"), 30, 0) +
    
    // Large uploads
    if(bytes_out > 10000000, 20, 0) +
    
    // Encoded PowerShell
    if(match(CommandLine, "(?i)-enc"), 25, 0)

// Aggregate by host
| stats sum(threat_score) as total_score, values(EventCode) as events by ComputerName OR src_ip

// Categorize threat level
| eval threat_level = case(
    total_score >= 100, "CRITICAL",
    total_score >= 50, "HIGH",
    total_score >= 25, "MEDIUM",
    total_score >= 10, "LOW",
    1=1, "INFO"
)

| where threat_score > 0
| sort -total_score
| table ComputerName, total_score, threat_level, events
```

### Use case: Prioritize investigation targets

