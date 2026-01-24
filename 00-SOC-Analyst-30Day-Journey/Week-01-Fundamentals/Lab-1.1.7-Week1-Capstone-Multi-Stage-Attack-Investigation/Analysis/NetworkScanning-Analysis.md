## PHASE 3: NETWORK RECONNAISSANCE

### Timeline:

11:00:00 - Network scanning initiated (Event 4688)
  Command: net view (Windows network discovery)
  
11:00:00 - 11:10:00 - Port scanning (PCAP analysis)
  Tool: Nmap (signature detected in PCAP)
  Target: 192.168.10.0/24 (entire subnet)
  Ports: 21, 22, 80, 443, 3306, 3389
  Results: 15 live hosts discovered

### Discovered Systems:

192.168.10.50 - FINANCE-WS01 (already compromised)
192.168.10.100 - WEB-APP-01 (next target)
192.168.10.200 - DB-SQL-01 (ultimate target)

### MITRE ATT&CK:

T1046 - Network Service Scanning
T1018 - Remote System Discovery