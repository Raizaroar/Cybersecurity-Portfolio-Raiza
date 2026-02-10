## STEP 2.2.1.6: Behavior-Based Detections

Move beyond signatures to behavior.

### **Detection 1: Unusual Process Graph**

Concept: Detect abnormal parent-child relationships

```spl
index="windows_security" EventCode=4688
| eval parent=lower(ParentProcessName)
| eval child=lower(NewProcessName)

// Create process relationship
| eval process_pair=parent + " -> " + child

// Count occurrences
| stats count by process_pair

// Find rare relationships (< 5 occurrences)
| where count < 5

// Exclude known-good patterns
| where NOT (
    process_pair LIKE "%explorer.exe -> %chrome.exe%" OR
    process_pair LIKE "%explorer.exe -> %firefox.exe%" OR
    process_pair LIKE "%services.exe -> %svchost.exe%"
)

| sort count
| head 20
```

**Why behavior-based:**

- Catches new malware (no signature needed)
- Detects fileless attacks
- Harder for attackers to evade


### **Detection 2: Anomalous Network Beaconing**

Concept: Identify C2 beaconing through timing analysis

```spl
index="firewall_logs" action="ALLOW"
| stats count by _time, src_ip, dest_ip span=1m

// Calculate time between connections
| streamstats current=f window=1 last(_time) as last_time by src_ip, dest_ip
| eval time_delta = _time - last_time

// Find consistent intervals (beaconing)
| stats stdev(time_delta) as time_variance, avg(time_delta) as avg_interval, count by src_ip, dest_ip

// Low variance + regular interval = beacon
| where time_variance < 10 AND avg_interval < 300 AND count > 10

| table src_ip, dest_ip, avg_interval, time_variance, count
| sort time_variance
```

**Beacon indicators:**

- Low time variance (consistent callback)

- Regular intervals (60s, 120s, etc.)

- Many connections over time


## **Detection 3: Data Staging Detection**

Concept: Find large file operations before exfiltration

```spl
index="windows_security" EventCode=4663 AccessMask="0x2"
| stats sum(eval(ObjectSize)) as total_bytes by User, ObjectName

// Convert to MB
| eval total_MB = total_bytes / 1048576

// Large data access
| where total_MB > 100

// Group by hour to find staging
| bin _time span=1h
| stats sum(total_MB) as hourly_MB by _time, User

| where hourly_MB > 500

| table _time, User, hourly_MB
```

**Exfiltration prep indicators:**

- Accessing many files quickly
- Large data volumes
- Unusual user behavior

