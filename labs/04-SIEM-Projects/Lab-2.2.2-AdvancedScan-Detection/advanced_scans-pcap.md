**Filter 1: SYN Scan Detection**

```bash
tcp.flags.syn==1 && tcp.flags.ack==0 && tcp.seq==0
```

**Why this filter works:**
- Nmap sends SYN with sequence number 0
- Legitimate clients use random seq numbers
- **Additional heuristic:** Window size

**Enhanced filter:**

```bash
tcp.flags.syn==1 && tcp.flags.ack==0 && tcp.window_size <= 1024
```

Nmap default window: 1024 bytes  
Most OS use 64240+ bytes

**What to look for:**
- **Statistics → Conversations → TCP**
- Sort by "Packets" (descending)
- Look for: Many packets, few bytes per packet

---

**Filter 2: FIN Scan Detection**

```bash
tcp.flags.fin==1 && tcp.flags.syn==0 && tcp.flags.ack==0
```

**Verification:**
- Right-click packet → Follow → TCP Stream
- Should see ONLY FIN, no prior SYN (incomplete conversation)

---

**Filter 3: XMAS Scan Detection**

```bash
tcp.flags.fin==1 && tcp.flags.push==1 && tcp.flags.urg==1
```

**In Wireshark packet details:**

```bash
Flags: 0x029 (FIN, PSH, URG)
    .... ...1 = Fin: Set
    .... .1.. = Push: Set
    ..1. .... = Urgent: Set
```

---

**Filter 4: NULL Scan Detection**

```bash
tcp.flags==0x000
```

**Alternative notation:**

```bash
tcp.flags==0
```

---

**Filter 5: ACK Scan Detection**

```bash
tcp.flags.ack==1 && tcp.flags.syn==0 && tcp.window_size==1024
```

**Distinguish from legitimate ACKs:**
- Legitimate ACK: Part of established connection (you'll see prior SYN/SYN-ACK)
- Scan ACK: Random sequence number, no prior handshake

---
