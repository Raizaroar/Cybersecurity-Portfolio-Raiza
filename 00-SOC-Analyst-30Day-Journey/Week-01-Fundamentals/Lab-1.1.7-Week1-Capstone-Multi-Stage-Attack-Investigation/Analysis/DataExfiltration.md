## PHASE 5: DATA THEFT

### Timeline:

13:00:00 - Database connection (PCAP)
  Source: WEB-APP-01 (192.168.10.100)
  Dest: DB-SQL-01 (192.168.10.200:3306)
  Protocol: MySQL
  
13:05:00 - Data extraction
  Query: SELECT * FROM customers
  Rows: 45,000 customer records
  
13:30:00 - File access (Event 4663)
  File: \\DB-SQL-01\SharedData\customer_database.xlsx
  User: jsmith (compromised account)
  
14:00:00 - Data exfiltration (PCAP)
  Source: FINANCE-WS01 (192.168.10.50)
  Dest: 185.220.101.45:443
  Data: 43.5 MB (ZIP archive, password protected)
  Duration: 10 minutes
  
### Stolen Data Assessment:

- 45,000 customer records
- Likely contains: Names, addresses, SSNs, credit cards
- Compliance Impact: GDPR, PCI-DSS violations
- Estimated financial impact: $5-10 million

### MITRE ATT&CK:

T1005 - Data from Local System
T1039 - Data from Network Shared Drive
T1020 - Automated Exfiltration
T1041 - Exfiltration Over C2 Channel
T1560.001 - Archive via Utility (ZIP)