# Suricata Rule Creation

If you need Edit Suricata rules:

1. Rule 1: SYN Scan Detection

```bash
alert tcp any any -> $HOME_NET any (msg:"Possible SYN Scan Detected"; flags:S,12; threshold:type threshold, track by_src, count 50, seconds 60; classtype:attempted-recon; sid:1000001; rev:1;)
```

### Rule breakdown:

```bash
alert tcp any any -> $HOME_NET any:
```

***Any source IP/port → Our network/any port***

```bash
msg:"Possible SYN Scan Detected":
```

***Alert message***

```bash
flags:S,12:
```

***S = SYN flag set***

***12 = Mask (only check SYN and ACK flags, ignore others)***

```bash
threshold:type threshold, track by_src, count 50, seconds 60:
```

- type threshold: Alert only ONCE when threshold is met
- track by_src: Track per source IP
- count 50: Trigger after 50 packets
- seconds 60: Within 60-second window

```bash
classtype:attempted-recon:
```

***Category (used for severity scoring)***


```bash
sid:1000001:
```

***Signature ID (1000000+ for custom rules)***

```bash
rev:1:
```

***Revision number***

2. Rule 2: XMAS Scan Detection

```bash
alert tcp any any -> $HOME_NET any (msg:"XMAS Scan Detected"; flags:FPU,12; classtype:attempted-recon; priority:1; sid:1000002; rev:1;)
```

**Why no threshold?**

- XMAS scan flag combination is NEVER legitimate
- Even ONE packet should trigger alert
- priority:1: Highest priority (1-4 scale, 1=highest)

3. Rule 3: NULL Scan Detection

```bash
alert tcp any any -> $HOME_NET any (msg:"NULL Scan Detected"; flags:0; classtype:attempted-recon; priority:1; sid:1000003; rev:1;)
```

**flags:0 means:**

- No flags set (all zeros)
- Also extremely rare in legitimate traffic

4. Rule 4: FIN Scan Detection

```bash
alert tcp any any -> $HOME_NET any (msg:"FIN Scan Detected"; flags:F,12; threshold:type threshold, track by_src, count 30, seconds 60; classtype:attempted-recon; sid:1000004; rev:1;)
```

**Why threshold 30 (lower than SYN's 50)?**

- FIN scans target FEWER ports (already know open ones)
- Lower threshold = earlier detection

5. Rule 5: ACK Scan Detection

```bash
alert tcp any any -> $HOME_NET any (msg:"ACK Scan - Firewall Mapping Attempt"; flags:A,12; threshold:type threshold, track by_src, count 20, seconds 60; classtype:attempted-recon; sid:1000005; rev:1;)
```

