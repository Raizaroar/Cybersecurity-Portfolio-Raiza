## Project Overview

Comprehensive training in phishing email analysis, covering email header inspection, sender authentication verification (SPF/DKIM/DMARC), malicious link/attachment identification, and IOC extraction. Phishing is the #1 attack vector globally (90% of breaches start with phishing), making this skill essential for all SOC analysts.

**Real-World Relevance:**  
Every SOC analyst receives daily reports of suspicious emails. This lab teaches the EXACT workflow used in production environments to triage, analyze, and respond to potential phishing threats.

## LAB ENVIRONMENT

- **Analysis System:** Kali Linux (safe, isolated)
- **Email Samples:** From PhishTank, public repositories
- **Tools:** Web-based (MXToolbox, URLScan) + CLI
- **Network:** Internet required for online tools
- **Safety:** NEVER click suspicious links or open attachments directly

## PREREQUISITES

- Lab 1.1.4 completed 
- Kali Linux VM running
- Internet connection
- Email client installed (Thunderbird recommended)
- Understanding of basic email concepts (From, To, Subject)

## Real-life scenario:

I'm an SOC Analyst and several users have reported suspicious emails:

***“Your Office 365 account will be suspended”***
***“Outstanding invoice - attached”***
***“Reset your bank password urgently”***

My job:

- Analyze email headers
- Verify sender authenticity
- Inspect links (without clicking)
- Analyze attachments (without executing them)
- Determine if it is phishing
- Extract IOCs and recommend actions

## STEP BY STEP - LABORATORY 1.1.5

### STEP 1.1.5.1: Understanding Email Anatomy

### Key Headers for Security Analysis

| Header             | Purpose                          | Phishing Indicator                          |
|--------------------|----------------------------------|---------------------------------------------|
| **From**           | Displayed sender                 | Can be spoofed easily                       |
| **Return-Path**    | Actual sender email              | Often reveals real attacker                 |
| **Reply-To**       | Where replies go                 | Different from "From" = suspicious          |
| **Received**       | Mail server hops                 | Shows email journey, can reveal origin      |
| **X-Originating-IP**| Sender's IP                     | Geographic location, reputation check       |
| **SPF**            | Sender Policy Framework          | FAIL = unauthorized sender                  |
| **DKIM**           | Domain Keys Identified Mail      | FAIL = message tampered/forged              |
| **DMARC**          | Domain-based Message Auth        | FAIL = domain being spoofed                 |


### Email Authentication Protocols

| Protocol | Purpose                                 | Example                                           | Result                                  |
|----------|------------------------------------------|---------------------------------------------------|------------------------------------------|
| **SPF**  | Verify sender is authorized to send from domain | `company.com` says "only these IPs can send our emails" |  PASS = legitimate<br> FAIL = spoofed |
| **DKIM** | Cryptographically sign emails            | Digital signature proves email wasn't tampered    | PASS = authentic<br> FAIL = modified/forged |
| **DMARC**| Define policy for failed SPF/DKIM        | "If SPF/DKIM fail, reject the email"              |  PASS = authentic<br> FAIL = spoofed |

## STEP 1.1.5.2: Install Email Analysis Tools

**Install Thunderbird (Email Client)** 

***Install Thunderbird***

```bash
sudo apt update
sudo apt install thunderbird -y

# Launch
thunderbird &
```

**Why Thunderbird?** Open-source, safe and Can open .eml files without account, also shows raw headers easily then No risk of auto-executing macros (unlike Outlook)

**Setup Email Header Analyzer:**

No installation needed, web-based:

- MXToolbox: https://mxtoolbox.com/EmailHeaders.aspx
- Google Admin Toolbox: https://toolbox.googleapps.com/apps/messageheader/
- Mail-Tester: https://www.mail-tester.com/

![1.1.5PishingAnalysis](/assets/screenshots/07-Threat-Intelligence/Lab-1.1.5-Phishing-Analysis/Lab-1.1.5-PhisingAnalysis.png)

**Install Additional CLI Tools:**

***Install mutt (CLI email client)***

```bash 
sudo apt install mutt -y
```

***Install swaks (SMTP testing tool)***

```bash
sudo apt install swaks -y
```

**Verify installations**

```bash 
thunderbird --version
mutt -v
```

## STEP 1.1.5.3: Analyze Sample Phishing Email #1 - Fake Microsoft

[Click Here: Microsoft Phishing Sample](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/labs/07-Threat-Intelligence/Lab-1.1.5-Phishing-Analysis/samples-phishing/email-microsoft.md)

 **Analysis Steps:**

1. **Visual Inspection:**

RED FLAGS IDENTIFIED:

**Subject:**

- "URGENT" in caps (creates panic)
- Threats of suspension (fear tactic)

**From Address:**

- "microsoft-security.com" (NOT microsoft.com)
- Missing official branding

**Body Content:**

- Generic greeting ("Dear Valued Customer")
- Spelling/grammar appears professional (modern phishing)
- Urgency/deadline (24 hours)
- Threat of account closure

**Link:**

- Domain: malicious-site.com (NOT microsoft.com)
- Suspicious subdomain: microsoft-secure-login
- Query parameter includes victim email (personalization attempt)

2. **Header Analysis Field on Thunderbird**


![1.1.5PishingAnalysis](/assets/screenshots/07-Threat-Intelligence/Lab-1.1.5-Phishing-Analysis/Lab-1.1.5-PhisingAnalysis3.png)

**Key Header Analysis:**


- **From: Microsoft Security Team <no-reply@microsoft-security.com>**

   - Domain: microsoft-security.com (typosquatting)
   - Real Microsoft: microsoft.com

- **Return-Path: attacker@malicious-server.ru**

   - CRITICAL: Actual sender is .ru domain (Russia)
   - Doesn't match "From" domain (spoofing)

- **Reply-To: support@fake-microsoft.net**

   - Yet another different domain
   - Replies go to attacker, not Microsoft

- **X-Originating-IP: 203.0.113.50**

   - Check IP reputation 

**SPF: FAIL**

   - Sender NOT authorized by microsoft-security.com

- **DKIM: FAIL**

   - Email signature invalid/missing

- **DMARC: FAIL**

   - Domain policy says REJECT this email


**Verdict after headers:** ***CONFIRMED PHISHING***

3. **IP Reputation Check:**

**IP Analyzed:** 203.0.113.50  
**Result:** This is a documentation IP (RFC 5737) used for examples.  
**Action Taken:** Proceeded to analyze other indicators from the phishing email.

**Note:** In a real scenario, I would extract actual IPs from email headers (Received fields) and verify them against threat intelligence databases like AbuseIPDB.

![1.1.5PishingAnalysis](/assets/screenshots/07-Threat-Intelligence/Lab-1.1.5-Phishing-Analysis/Lab-1.1.5-PhisingAnalysis4.png)

### Example

| Field                  | Value                        |
|-------------------------|------------------------------|
| Abuse Confidence Score | 100%                         |
| Total Reports          | 150+                         |
| Country                | Russia                       |
| ISP                    | Malicious Hosting Provider   |
| Last Reported          | 2 hours ago                  |
| Categories             | Email Spam, Phishing         |

4. **MXToolbox Blacklist Check:**

**IP Analyzed:** 203.0.113.50  
**Result:** This is a documentation IP (RFC 5737) used for examples.  

![1.1.5PishingAnalysis](/assets/screenshots/07-Threat-Intelligence/Lab-1.1.5-Phishing-Analysis/Lab-1.1.5-PhisingAnalysis5.png)


5. URL Analysis

***Use URLScan.io:***

https://microsoft-secure-login.malicious-site.com/verify?id=victim@company.com

**URLScan Results:**

![1.1.5PishingAnalysis](/assets/screenshots/07-Threat-Intelligence/Lab-1.1.5-Phishing-Analysis/Lab-1.1.5-PhisingAnalysis6.png)


**Domain Analysis:**

- Domain: malicious-site.com
- Registered: 2 days ago ( brand new)
- Registrar: Privacy-protected ( hidden owner)
- Hosting: Bulletproof hosting ( known for abuse)

**Page Content:**

- Fake Microsoft login page
- Credential harvesting form
- No HTTPS certificate (insecure)
- Visual similarity to real Microsoft: 85%

**Contacted IPs:**

- 185.53.179.200 (malicious server)

**Final Determination:**

- Phishing site (credential theft)
- Brand impersonation (Microsoft)

Verdict: MALICIOUS

5. IOCs - Phishing Email #1 (Fake Microsoft)

**Email IOCs**

- Sender Domain: microsoft-security.com (BLOCK)
- Actual Sender: attacker@malicious-server.ru (BLOCK)
- Reply-To: support@fake-microsoft.net (BLOCK)

**Network IOCs**

- Originating IP: 203.0.113.50 (BLOCK at firewall)
- Phishing Site IP: 198.51.100.25 (BLOCK at proxy)
- Phishing Domain: malicious-site.com (BLOCK DNS)
- Phishing URL: microsoft-secure-login.malicious-site.com (BLOCK)

**Subject Line Indicators**

- "[URGENT] Your Microsoft account will be suspended"

**Behavioral Indicators**

- SPF: FAIL
- DKIM: FAIL
- DMARC: FAIL
- Typosquatting domain
- Urgency language
- Credential harvesting page