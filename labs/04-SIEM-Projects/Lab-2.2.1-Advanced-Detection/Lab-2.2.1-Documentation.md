##  LAB 2.2.1: Detection Engineering Deep Dive - YARA & Advanced Rules

## PROJECT OVERVIEW

Comprehensive detection engineering covering YARA malware detection, behavior-based analytics, threat scoring systems, and detection maturity assessment. Created 4 YARA rules, 5 behavior-based detections, implemented threat scoring, and built automated testing framework.

**Real-World Context:**

Detection Engineers are the “architects” of the SOC. While analysts respond to alerts, engineers create the rules that generate those alerts.

## LAB ENVIRONMENT

- Primary System: Kali Linux
- SIEM: Splunk (from previous labs)
- Malware Samples: Safe test files (EICAR, custom benign)

## PREREQUISITES

- Labs 2.1.1 & 2.1.2 completed 
- Kali Linux VM operational
- Splunk running with data
- Understanding of MITRE ATT&CK

STEP 2.2.1.1: Install YARA on Kali Linux

1. Install YARA:

![Lab2.2.1-AdvancedDetection](/assets/screenshots/04-SIEM-Projects/Lab-2.2.1-Advanced-Detection/Lab-2.2.1-AdvancedDetection1.png)

2. Install Python YARA module on virtual Enviroment 

```bash
sudo apt install python3-venv 
python3 -m venv ~/venv-yara 
source ~/venv-yara/bin/activate 
pip install yara-python
```

**Verify**

![Lab2.2.1-AdvancedDetection](/assets/screenshots/04-SIEM-Projects/Lab-2.2.1-Advanced-Detection/Lab-2.2.1-AdvancedDetection2.png)

3. Install yarGen (YARA rule generator):

```bash
# Clone yarGen repository
cd ~/tools
git clone https://github.com/Neo23x0/yarGen.git
cd yarGen

# Install dependencies
pip3 install -r requirements.txt

# Update database (downloads good file signatures)
python3 yarGen.py --update
```

![Lab2.2.1-AdvancedDetection](/assets/screenshots/04-SIEM-Projects/Lab-2.2.1-Advanced-Detection/Lab-2.2.1-AdvancedDetection3.png)



## STEP 2.2.1.2: YARA Fundamentals

1. YARA Rule Anatomy:

```yara
rule RuleName
{
    meta:
        description = "What this rule detects"
        author = "Raiza"
        date = "2025-01-30"
        reference = "https://attack.mitre.org/..."
        
    strings:
        $string1 = "malicious_text" ascii wide
        $hex1 = { 6A 40 68 00 30 00 00 }
        $regex1 = /http:\/\/badsite\.com\/[a-z]{8}\.exe/
        
    condition:
        any of them
}
```

#### Components:

- **meta:** Documentation (not used in matching)

- **strings:** Patterns to search for

- **condition:** Logic to trigger match

1. My First YARA Rule

```yara
rule EICAR_Test_File
{
    meta:
        description = "Detects EICAR test file"
        author = "Raiza"
        date = "2025-01-30"
        severity = "low"
        reference = "https://www.eicar.org/"
        
    strings:
        $eicar = "X5O!P%@AP[4\\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*"
        
    condition:
        $eicar
}
```


2. Create EICAR test file:

Key features

- **Purpose:** used to check whether an antivirus or detection system reacts correctly to a simulated threat.

- **Content:** contains a specific text string (the EICAR test string) that is harmless, but which all antivirus programs recognize as a virus.

- **Security:** completely harmless; does not execute malicious code or compromise the system.

```bash
# Create test file
cd ~/lab-yara
echo 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*' > eicar.txt
```

3. Test YARA rule:

```bash
# Scan file with rule
yara rules/test_rule.yar eicar.txt

# Expected output:
# EICAR_Test_File eicar.txt

# Scan entire directory
yara -r rules/test_rule.yar .
```


![Lab2.2.1-AdvancedDetection](/assets/screenshots/04-SIEM-Projects/Lab-2.2.1-Advanced-Detection/Lab-2.2.1-AdvancedDetection4.png)

***Success! I've created and executed your first YARA rule***

## STEP 2.2.1.3: Advanced YARA Rules

1. Rule 1: Detect Suspicious PowerShell


```yara
rule Suspicious_PowerShell_Encoding
{
    meta:
        description = "Detects base64-encoded PowerShell commands"
        author = "Raiza"
        date = "2025-01-30"
        severity = "high"
        mitre_attack = "T1059.001"
        
    strings:
        // Base64 encoded common malicious commands
        $enc1 = "JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAA" ascii wide  // $s=New-Object
        $enc2 = "SQBuAHYAbwBrAGUALQBXAGUAYgBSAGUAcQB1" ascii wide  // Invoke-WebRequest
        $enc3 = "SQBuAHYAbwBrAGUALQBFAHgAcAByAGUAcwBz" ascii wide  // Invoke-Expression
        
        // Common obfuscation patterns
        $pattern1 = /-[Ee]ncodedcommand/ nocase
        $pattern2 = /-[Ee]nc / nocase
        $pattern3 = /IEX\s*\(/ nocase
        $pattern4 = /DownloadString/ nocase
        
    condition:
        any of ($enc*) or 2 of ($pattern*)
}
```

***Designed to detect malicious PowerShell commands that have been obfuscated or encoded in Base64***

2. Rule 2: Detect Mimikatz (Credential Dumping)

```yara
rule Mimikatz_Credential_Dumper
{
    meta:
        description = "Detects Mimikatz credential dumping tool"
        author = "Raiza"
        date = "2025-01-30"
        severity = "critical"
        mitre_attack = "T1003.001"
        reference = "https://github.com/gentilkiwi/mimikatz"
        
    strings:
        // Mimikatz specific strings
        $s1 = "gentilkiwi" ascii wide
        $s2 = "mimikatz" ascii wide nocase
        $s3 = "sekurlsa::logonpasswords" ascii wide
        $s4 = "lsadump::sam" ascii wide
        $s5 = "privilege::debug" ascii wide
        
        // Function exports
        $f1 = "kuhl_m_sekurlsa_" ascii
        $f2 = "kuhl_m_lsadump_" ascii
        
        // Unique bytes patterns
        $hex1 = { 4D 69 6D 69 6B 61 74 7A }  // "Mimikatz" in hex
        
    condition:
        3 of ($s*) or any of ($f*) or $hex1
}
```

***Designed to detect the presence of Mimikatz, one of the most well-known tools for credential dumping (stealing credentials) on Windows systems.***

3. Rule 3: Detect Suspicious File Extensions

```yara 
rule Suspicious_Double_Extension
{
    meta:
        description = "Detects files with suspicious double extensions"
        author = "Raiza"
        date = "2025-01-30"
        severity = "medium"
        mitre_attack = "T1036.007"
        
    strings:
        // Double extensions used to trick users
        $ext1 = ".pdf.exe" nocase
        $ext2 = ".doc.exe" nocase
        $ext3 = ".xls.exe" nocase
        $ext4 = ".jpg.exe" nocase
        $ext5 = ".txt.scr" nocase
        $ext6 = ".zip.exe" nocase
        
    condition:
        filename matches /\.(pdf|doc|xls|jpg|txt|zip)\.(exe|scr|bat|cmd|vbs|js)$/i
        or any of them
}
```

***focused on detecting files with suspicious double extensions, a very common technique used by attackers to trick users into executing malware disguised as legitimate documents.***

4. Rule 4: Detect Cobalt Strike Beacon

```yara
rule CobaltStrike_Beacon
{
    meta:
        description = "Detects Cobalt Strike Beacon payload"
        author = "Raiza"
        date = "2025-01-30"
        severity = "critical"
        mitre_attack = "T1071.001"
        reference = "https://www.cobaltstrike.com/"
        
    strings:
        // Cobalt Strike beacon configuration markers
        $mz = "MZ"
        
        // Common C2 configuration strings
        $c2_1 = "Mozilla/5.0 (compatible; MSIE" ascii
        $c2_2 = "%s.4%08x%08x%08x%08x%08x.%08x%08x%08x%08x%08x%08x%08x.%08x%08x%08x%08x%08x%08x%08x.%08x%08x%08x%08x%08x%08x%08x.%x%x.%s" ascii
        
        // Beacon specific bytes
        $beacon1 = { 69 68 69 68 69 6B }
        $beacon2 = { 2E 2F 2E 2F 2E 2C }
        
        // Reflective loader patterns
        $reflective = { 4D 5A 41 52 55 48 89 E5 48 81 EC 20 00 00 00 48 8D 1D EA FF FF FF 48 81 C3 }
        
    condition:
        uint16(0) == 0x5A4D and  // MZ header
        filesize < 5MB and
        (
            any of ($c2_*) or
            any of ($beacon*) or
            $reflective
        )
}
```

***Designed to detect Cobalt Strike Beacon payloads, one of the tools most commonly used by advanced attackers and network teams to execute commands, establish communication with command and control (C2) servers, and move laterally within a network.***

