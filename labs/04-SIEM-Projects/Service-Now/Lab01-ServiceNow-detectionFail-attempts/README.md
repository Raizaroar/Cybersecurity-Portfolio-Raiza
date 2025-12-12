# Detection and correlation of unauthorized access attempts.

## Project Overview 

This lab simulates a real SOC scenario where we detect multiple failed SSH login attempts on a critical server. You will learn how to correlate events between system logs, Splunk, and ServiceNow to identify and document a potential brute force attack.

-**Duration**: 

1 hour
Difficulty: Entry Level
Platform: Kali Linux,

**Skills demonstrated:**

1. Authentication log analysis
2. Security event correlation
3. Incident management with ServiceNow
4. Query and visualization in Splunk


**Tools Used**

   - Kali Linux  - Analysis and simulation platform
   - Splunk Enterprise - SIEM for log correlation and analysis
   - ServiceNow - Incident management platform (ITSM)
   - SSH - Target service for simulating attacks
   - Hydra - Tool for simulating brute force attacks (for educational purposes)

**Main Objective:**
   - Detect, analyze, and document an SSH brute force attack using data correlation from multiple sources.

**Specific Objectives:**

   - Generate controlled SSH attack traffic.
   - Ingest logs into Splunk for analysis.
   - Create detection queries in Splunk.
   - Document the incident in ServiceNow with evidence.
   - Establish detection metrics (detection time, number of correlated events).

**Lab Environment**
Simulated attacker: Kali Linux (your machine)
Target: SSH server (can be localhost or another VM)
Monitoring: Splunk with Universal Forwarder
Management: ServiceNow (Incident Management)


**Prerequisites**

   - Basic navigation in Linux terminal
   - Basic SSH concepts
   - Understanding what a system log is

Splunk configured to receive SSH logs:

   - Have an active Splunk account 
   - Configure an input to receive logs from ```/var/log/auth.log```


ServiceNow Developer Instance:

   - Create a free account at: https://developer.servicenow.com/
   - Request a personal instance (takes ~30 seconds)


Kali Linux:

Have the following installed **hydra, ssh, curl**

## Phase 1 Preparation of the Environment

**Step 1.1 - Verify/Install tools in Kali**

Open the terminal in Kali and run:

```bash
sudo apt update && sudo apt install -y hydra openssh-server curl
```

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts.png)

***Why this command?***


```apt update``` Updates the list of available packages
```hydra``` Tool to simulate brute force attacks (controlled)
```openssh-server``` To have an SSH server to attack
```curl``` To make HTTP requests to the ServiceNow API

***Why this path?***

 I needee both the attacker (Hydra) and the target (SSH server) on the same machine for a controlled and secure lab.

**Step 1.2 - Start SSH service**

```bash
sudo systemctl start ssh
sudo systemctl status ssh
```

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts1.png)

***Why this command?***

```systemctl start ssh``` Starts the SSH service on my Kali

```systemctl status ssh``` Verifies that it is running correctly

**Step 1.3 Create test user**

```bash
sudo useradd -m testuser
echo “testuser:SecurePass123!” | sudo chpasswd
```

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts2.png)

***Why this command?***

```useradd -m``` Creates a new user with home directory

```chpasswd``` Sets the password securely

```-m``` flag Creates a “home” directory for the user

### Technical Challenge: Bash Special Characters

**Initial attempt failed due to:**

- The `!` character triggers history expansion in bash when using double quotes

- Error: `Authentication token manipulation error`

**Solution implemented:**
Used escape character `\` to treat `!` as literal:

```bash
echo "testuser:SecurePass123\!" | sudo chpasswd
```

**Alternative approaches considered:**

1. Single quotes: `echo 'testuser:SecurePass123!'` (simplest)
2. Interactive: `sudo passwd testuser` (most secure)
3. Escape character: `\!` (chosen for automation capability)

**Why this matters:**
Understanding shell metacharacters is critical for:

- Writing secure automation scripts
- Avoiding command injection vulnerabilities
- Proper password handling in scripts

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts3.png)

## PHASE 2: Configure Splunk to Receive Logs 

**Step 2.1 - Configure input in Splunk**

This is importan because:

   - Linux logs are stored in text files in /var/log/.
   - Splunk is a SIEM that “reads” these files and converts them into searchable events.
   - Without this configuration, Splunk does NOT know which files to monitor.

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts4.png)

```bash
sudo /opt/splunk/bin/splunk add monitor /var/log/auth.log -index main -sourcetype linux_secure -hostname kali-lab
```

1. ```sudo /opt/splunk/bin/splunk```  Splunk executable with elevated permissions

2. ```add monitor``` Adds a new “monitor” type input and  **What is monitor?:** Splunk “watches” the file in real time, Every time a new line is written, Splunk reads it immediately

3. ```/var/log/auth.log``` The path to the file to be monitored and  This is the “data source”

4. ```-index main```  It is like a “database” or “container” for events and  Splunk organizes data into separate indexes

   - If I do not specify it, Splunk uses “default”

   - It is best practice to be explicit

5. ```-sourcetype linux_secure``` It tells Splunk HOW to interpret the data and Each type of log has a different format because  Splunk needs to know the format in order to parse it correctly

```linux_secure```  It is Splunk's predefined sourcetype for Linux authentication logs and Splunk already knows how to extract fields (timestamp, user, action, etc.) from this format

6. ```-hostname kali-lab``` Assigns a “friendly name” to the host that generates the logs and it's important to change it because  In companies, you have 100+ servers and Descriptive names help you quickly identify them

- `kali-lab` is clearer than just “kali”

**COMMAND VERIFICATION**

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts5.png)

**STEP 2.5: Verify that Splunk is receiving data**


Navigate to Search & Reporting and In the search bar, run this query:

```spl
index=main sourcetype=linux_secure earliest=-5m
```

BREAKDOWN OF THE SPL QUERY:

SPL = Splunk Processing Language
Basic syntax: ```field=value``` ```field2=value2```
Query part by part:

1. ```index=main```

***What it does*** Searches only in the “main” index

***Why*** Limits the search scope (faster) Without this, Splunk would search ALL indexes

2. ```sourcetype=linux_secure```

***What it does*** Filters only events that are Linux authentication logs

***Why*** There could be other data in main (web logs, application logs, etc.)

***Implicit operator*** When you set multiple conditions, the default operator is AND.

***Equivalent*** index=main AND sourcetype=linux_secure

3. ```earliest=-5m```last 5 minutes

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts6.png)

**STEP 2.6: Generate Test Events**

***Why generate test events?***

- To CONFIRM that Splunk is reading in real time
- Create controlled test data
- Verify that the entire pipeline is working

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts7.png)


 **STEP 2.6: Explore the Data in Splunk**

```spl
 index=main sourcetype=linux_secure 
| table _time, host, user, app, src_ip
```

   - ```table``` command

**What does it do?**
- Creates a table with the specified columns

- Similar to `SELECT` in SQL

**Specified fields:**

   - ```_time```

- Event timestamp the underscore is a Splunk internal fields begin with `_`

   - ```host```

- System hostname

   - ```user```
- User involved in the event (automatically extracted by `linux_secure` sourcetype)

   - ```app```
- Application that generated the log (ssh, su, sudo, etc.)

   - ```src_ip```
- Source IP (if applicable)
- For remote SSH, it would show the client's IP
- For localhost (our case), it could be empty

I can see a clean table with sorted columns showing the events.

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts8.png)

## PHASE 3: EXECUTION OF THE SIMULATED SSH ATTACK

 **STEP 3.1: Prepare the Password Dictionary**
 
***What is a password dictionary?***

   - It is a list of common passwords that an attacker automatically tries.
   - Real attackers use dictionaries with millions of entries.
   - I will use a small one (7 passwords) for educational purposes.

***Why do attackers use dictionaries?***

   - People use predictable passwords (123456, password, admin, etc.) 
   - It is more efficient than trying random combinations
   - Dictionary-based attacks are very common in real life

**STEP 3.2 Created my dictionary**


```bash
cd ~/Desktop
mkdir ssh-bruteforce-lab
cd ssh-bruteforce-lab

cat > passwords.txt << EOF
admin123
password
testuser
123456
wrongpass1
wrongpass2
SecurePass123!
EOF
```

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts9.png)


Why this command?

```cat > file << EOF``` Creates a file with multiple lines of content.
I included the actual password ---(SecurePass123!)--- at the END so that Hydra generates many failed attempts first.

```<< EOF``` It tells the shell: “what comes next is INPUT for the command.” Everything you type until you find EOF will be passed as input.

***What is EOF?***

End Of File = End of file, It is a “delimiter” (end marker), You can use any word, but EOF is the convention.


***Why this approach?*** 

I want to simulate a realistic attack where the attacker tries many combinations before finding the correct one.


**Verify that the file was created correctly**

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts10.png)

**STEP 3.3: Verify Local SSH Connectivity**

 ***Why check before attacking?***

   - If SSH isn't working normally, the attack won't work either.
   - Troubleshooting principle: check the basics first.
   - Avoid wasting time attacking an inaccessible target.

```bash
ssh testuser@localhost
```

**STEP 3.3: RUN HYDRA - THE ATTACK**

***The attack command***

```bash
hydra -l testuser -P passwords.txt ssh://127.0.0.1 -t 4 -V
```

COMMAND BREAKDOWN

```hydra``` is the brute force tool for multiple protocols.

Why Hydra vs. other tools?
- Medusa: Similar, less used.
- Ncrack: Part of Nmap, slower.
- Metasploit modules: Heavier.

```-l testuser``` This part specifies the target login (username) -l = login

-  -l for a single user
   -L file.txt for a list of users (multiple)

***Why -l (one user) vs -L (list)?***

- We know the user is testuser
- We are only testing different passwords

In real attacks:

- You could test multiple users AND multiple passwords
- Example: -L users.txt -P passwords.txt = Test all combinations

```-P passwords.txt``` Specifies the file with passwords to test
-P (uppercase) = list of passwords from file
-p (lowercase) = ONE single password

```ssh://127.0.0.1``` specifies the protocol and target

- Structure: protocol://host[:port]
Breakdown:

```ssh://``` Tells Hydra to attack the SSH service. Hydra supports 50+ different protocols. Examples: ftp://, http://, rdp://, mysql://

```127.0.0.1``` The IP address of the target, since 127.0.0.1 = localhost (this same machine)

- Thread selection rationale ```-t 4```, Chose 4 parallel threads to balance attack speed with system and stability. Higher thread counts (16+) can:
- Overload target resources (DoS risk)
- Trigger rate-limiting protections
- Create obvious detection patterns

4 threads simulates a "careful" attacker who wants to avoid 
detection while maintaining efficiency.

Why do we use -V? I can see what's happening in real time and Troubleshooting If something goes wrong, you know on which attempt

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts11.png)


**Attack summary**

- 1 target attacked
- 1 valid password found
- Attack completed

```bash
sudo tail -30 /var/log/auth.log | grep testuser
```

What does this command do?
- ```tail -30``` Last 30 lines of the log
- ```| grep testuser``` Filters only lines with “testuser”
- Displays login attempts recorded by the system

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts12.png)

**STEP 3.5: VERIFY IN SPLUNK**
 
***Why verify in Splunk NOW?***

- To see how a SIEM detects the attack in real time
- This is how SOC analysts work and they monitor dashboards while events are occurring

```spl
index=main sourcetype=linux_secure user=testuser earliest=-5m
```

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts13.png)


```spl 
index=main sourcetype=linux_secure user=testuser earliest=-5m
| timechart span=10s count by action
```


**What does this do?**

timechart: Time chart
span=10s: 10-second windows
Shows how many attempts per 10 seconds

Purpose: To visualize the “speed” of the attack

**KEY CONCEPTS LEARNED**

1. Brute force attack pattern:
Detectable characteristics:

- Multiple consecutive failed attempts
- Same source IP
- Same target user
- Very close timestamps (automation)
- Possible success after failures

**Difference from human errors**

- Human user: 1-3 attempts, spaced out (minutes)
- Automated attack: Dozens/hundreds of attempts, rapid (seconds)


2. Indicators of Compromise (IOCs):

- Source IP: 127.0.0.1 (in a real environment, it would be an external IP)
- Target User: testuser
- Attack Pattern: 6 failed + 1 success
- Timeframe: ~10-30 seconds
- Service: SSH (port 22)

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts14.png)


## PHASE 4: DATA ANALYSIS AND CORRELATION IN SPLUNK


**STEP 4.1: Basic Detection Query**

Initial query - View all attack events, In Splunk Search, run:

```spl
index=main sourcetype=linux_secure user=testuser earliest=-1h
```

Visual observation:
Look at the timeline, I can see **a spike** at a specific moment. That spike represents the concentrated attack.

***Why is the spike important?***

It shows a temporary anomaly.
- Normal behavior: spaced-out events.
- Automated attack: events concentrated within seconds.



### Attack Pattern Analysis

**Quantitative findings:**
- Failed authentication attempts: 6
- Successful authentication: 1
- Success rate: 14.3% (1/7 attempts)
- Attack pattern: Multiple failures followed by success

**Significance:**
This pattern is highly indicative of automated brute force attack:
- Legitimate user typically succeeds within 1-2 attempts
- 6 consecutive failures suggests password guessing
- Successful breach after failures = potential compromise

**Risk assessment:** HIGH
- Account potentially compromised
- Credentials may now be in attacker's possession


**4.2 Temporal Correlation Query** 

Why does temporal correlation matter? Because the TIME between events is key to distinguishing attacks from human errors.
A user who forgets their password: attempt → wait 5 minutes → attempt.
An automated attacker: attempt → attempt → attempt (seconds apart).

![Serviceno1](../../../../assets/screenshots/04-SIEM-Projects/Service-Now/Lab01-ServiceNow-detectionFail-attempts/Lab01-ServiceNow-detectionFail-attempts15.png)






### Legal and Ethical Disclaimer

This penetration testing activity was conducted:
- On my own system (Kali Linux VM)
- In an isolated lab environment
- Against accounts I created and own
- For educational purposes only

**I understand that:**
- Unauthorized access to systems is illegal (CFAA, Computer Fraud and Abuse Act)
- These tools must ONLY be used on systems I own or have explicit written authorization to test
- Professional penetration testers obtain signed contracts before testing

