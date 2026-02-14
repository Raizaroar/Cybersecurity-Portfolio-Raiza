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