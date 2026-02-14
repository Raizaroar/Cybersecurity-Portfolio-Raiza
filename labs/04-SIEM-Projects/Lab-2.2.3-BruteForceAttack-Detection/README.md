# DAY 12 - BRUTE FORCE ATTACK DETECTION & EVENT CORRELATION 

**Status:** Completed

**Category:** SIEM

**Difficulty:** Intermadiate - Advanced

**Estimated Time:** 6 hours

**Environment:** Kali Linux + Metasploitable lab setup


## Executive Summary

Detected and analyzed a **complete attack chain** from reconnaissance (port scanning) to exploitation (brute force breach). Successfully correlated 2,547 failed login attempts leading to confirmed account compromise. Implemented automated defense (Fail2Ban) and created correlation engine for SIEM integration.

## Objective

- Correlated scan → brute force → breach
- Reconstruct the attacker's complete timeline
- Understand MITRE ATT&CK kill chain

## Tools & Techniques

| Tool | Purpose | Key Feature |
|------|---------|-------------|
| **Hydra** | Brute force tool | Multi-threaded password attacks |
| **Python** | Log analysis | Regex parsing, correlation engine |
| **Fail2Ban** | Automated defense | IP banning after threshold |
| **Wireshark** | Traffic analysis | Captured auth attempts |


### Steps
1. **Reconnaissance:** `nmap -sS 10.0.2.4 -p 22`
2. **Brute Force:** `hydra -L users.txt -P passwords.txt ssh://10.0.2.4`
3. **Collect Logs:** `scp target:/var/log/auth.log ./`
4. **Analyze:** `python3 correlation_engine.py auth.log`
5. **Visualize:** Open `Day12_Analysis.ipynb`

## Key Learnings

 1. Correlation is Critical

- Individual events (scan, brute force) might be noise
- **Correlated events = High-confidence attack**

 2. Timing Reveals Intent

- 8-minute gap between scan → brute force = automated attack
- Legitimate users don't scan before logging in

3. Defense in Depth

- **Detection:** Log monitoring (what we did)
- **Prevention:** Fail2Ban (automated response)
- **Hardening:** Strong passwords, 2FA, rate limiting

 4. False Positive Reduction

- One failed login = Normal user error
- 10+ failed logins in 5 min = Suspicious
- 100+ failed logins = Definite attack



## Attack Kill Chain (MITRE ATT&CK)

```
Phase 1: Reconnaissance (T1046)
  └─> Port scan discovered SSH (22) open

Phase 2: Initial Access (T1110.001 - Password Guessing)
  └─> Brute force attack: 2,547 attempts in 15 min
  └─> Rate: 165 attempts/minute

Phase 3: Credential Access (T1110)
  └─> SUCCESS: Credentials found (msfadmin/msfadmin)

Phase 4: Valid Accounts (T1078)
  └─> Attacker gained legitimate access
```

## Skills Demonstrated

- Log analysis (Linux auth logs)
- Event correlation
- Attack chain reconstruction
- MITRE ATT&CK mapping
- Python automation
- Regex pattern matching
- Defensive tool configuration (Fail2Ban)
- Incident timeline documentation

## CONCLUSION

**What You Accomplished:**

- **Executed complete attack chain** (Scan → Brute Force → Breach)  
- **Built correlation engine** to connect Day 11 (scans) with Day 12 (brute force)  
- **Analyzed 2,547 failed authentication attempts**  
- **Detected successful account compromise**  
- **Implemented automated defense** (Fail2Ban)  
- **Created MITRE ATT&CK mapped timeline**  
- **Generated SIEM-ready IOCs in JSON**

**Next Step: Lab Day 13**