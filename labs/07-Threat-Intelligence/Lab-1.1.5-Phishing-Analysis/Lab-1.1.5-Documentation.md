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

