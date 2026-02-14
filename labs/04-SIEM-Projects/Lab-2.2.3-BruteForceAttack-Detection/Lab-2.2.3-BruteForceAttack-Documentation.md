 #  BRUTE FORCE ATTACK DETECTION & EVENT CORRELATION


## Project Overview: 

The Intrusion Detection & Prevention Lab has the following objectives: to implement and validate a detection workflow for brute force attacks against critical services (SSH and FTP) using open-source tools and custom correlation logic. The aim was to measure the accuracy of the detection, the response time and the resilience against real-world attack scenarios.

## PHASE 1: Understanding the Attack Chain

### MITRE ATT&CK Mapping

**Attack Kill Chain**

1. RECONNAISSANCE  

- **Technique:** T1046 - Network Service Discovery  
    - **Action:** Port Scan → `nmap -sS 10.0.2.4 -p 22,21`  
    -  **Result:** SSH (22) OPEN, FTP (21) OPEN  

---

2. INITIAL ACCESS  

- **Technique:** T1110.001 - Password Guessing  
    - **Action:** Brute Force → `hydra -L users.txt -P pass.txt`  
    - **Target:** SSH service on `10.0.2.4:22`  

---

3. CREDENTIAL ACCESS  

- **Technique:** T1110 - Brute Force  
    - **Action:** 2,547 login attempts in 15 minutes  
    - **Success:** Credentials obtained → `user: msfadmin / password: msfadmin`  

---

4. PERSISTENCE  

- **Technique:** T1078 - Valid Accounts  
    - **Action:** Attacker now has legitimate credentials  


***Why this matters:***

1. Understanding the kill chain helps you detect attacks at MULTIPLE stages
2. If I miss the port scan, I can still catch the brute force
3. If I miss both, I can detect the successful login from suspicious IP

## PHASE 2: Environment Setup

1. Step 2.1: Prepare Metasploitable2 (Target )

```bash
# Verify SSH is running
sudo /etc/init.d/ssh start

# Check if SSH daemon is running:
ps aux | grep ssh

# Test local connection:
ssh localhost

# Check current auth log
sudo tail -f /var/log/auth.log
```

![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack.png)

***Leave this terminal open to watch real-time attacks***

2. Step 2.2: Prepare Ubuntu SOC 

Start packet capture:

```bash
sudo tcpdump -i enp0s3 -w brute_force.pcap host 10.0.2.4
```

Stop capture and Verify File 

```bash 
ls -lh brute_force.pcap
```

![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack2.png)

## PHASE 3: Reconnaissance (Port Scan)

1. Step 3.1: Port Scan from Kali 

```bash
# From Kali Linux
# First, discover open services
sudo nmap -sS 10.0.2.4 -p 1-1000 --open -oN reconnaissance.txt
```

![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack3.png)



**Attacker's thinking:**

- "SSH (22) is open → I can try brute force"
- "FTP (21) is open → Another brute force target"
- "Multiple services → Multiple attack vectors"

2. Step 3.2: Service Version Detection

```bash 
# More detailed scan to identify exact versions
sudo nmap -sV -p 22,21 10.0.2.4 -oN service_versions.txt
```

![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack4.png)

***Why this matters:***

- OpenSSH 4.7p1 → Vulnerable to user enumeration
- vsftpd 2.3.4 → Known backdoor

## PHASE 4: Brute Force Attack

1. Step 4.1: Create Username List


![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack5.png)

***Why these usernames?***

- admin, root, user → **Common defaults**
- msfadmin → **Metasploitable default user**
- service, postgres, tomcat → **Common service accounts**

2. Step 4.2: Create Password List

![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack6.png)

**In real attacks:**

- Attackers use rockyou.txt (14 million passwords)
- Or custom wordlists based on target research
- Or hybrid attacks (dictionary + rules)

3. Step 4.3: **Launch Brute Force with Hydra**

```bash
# SSH Brute Force
hydra -L users.txt -P passwords.txt ssh://10.0.2.4 -t 4 -V
```

**Parameters explained:**

- `-L users.txt` → Username list
- `-P passwords.txt` → Password list
- `ssh://10.0.2.4` → Target service
- `-t 4` → 4 parallel threads (not too aggressive to avoid detection)
- `-V` → Verbose (show each attempt)


![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack7.png)

> NOTE: In this part, I changed the ssh settings
```bash
# Fix MAC algorithms:***
nano ~/.ssh/config

# Add

Host 10.0.2.4
    MACs hmac-md5,hmac-sha1,hmac-ripemd160
```

4. Step 4.4: **FTP Brute Force** ***(Additional Attack Vector)***

```bash
# FTP Brute Force
hydra -L users.txt -P passwords.txt ftp://10.0.2.4 -t 4 -V
```

![Lab.2.2.3-BruteForce-Attack](/assets/screenshots/04-SIEM-Projects/Lab-2.2.3-BruteForceAttack-Detection/Lab-2.2.3-BruteForceAttack8.png)

***Why attack multiple services?***

- Increases chances of success
- Different services might have different password policies
- Shows persistence and determination


## PHASE 5: Detection & Analysis

1. Step 5.1: Analyze Auth Logs on Metasploitable

```bash
# View failed SSH attempts
sudo grep "Failed password" /var/log/auth.log | tail -20
```



- **Red flags:**

    - Multiple failed attempts from same IP (10.0.2.5)
    - Sequential port numbers (45678, 45679, 45680...)
    - Trying different usernames systematically
    - Final successful login after many failures

2. Step 5.2: Count Failed Attempts

