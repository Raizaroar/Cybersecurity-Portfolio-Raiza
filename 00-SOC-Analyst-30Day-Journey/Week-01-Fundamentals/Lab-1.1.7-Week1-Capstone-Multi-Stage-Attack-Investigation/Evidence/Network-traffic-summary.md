PCAP Analysis Summary - capture.pcapng

Time Range: 2025-01-22 09:30:00 - 14:30:00

TOP CONVERSATIONS – Network Traffic Summary

| Source IP       | Destination IP     | Packets | Bytes      | Protocol       |
|-----------------|--------------------|---------|------------|----------------|
| 192.168.10.50   | 185.220.101.45     | 4,567   | 348,234     | TCP (HTTPS)    |
| 192.168.10.50   | 192.168.10.100     | 892     | 67,123      | HTTP           |
| 192.168.10.100  | 192.168.10.200     | 2,345   | 123,456     | MySQL          |
| 192.168.10.50   | 185.220.101.45     | 8,901   | 45,678,912  | TCP (HTTPS)    |


SUSPICIOUS TRAFFIC:

1. Initial C2 Connection (09:32:00)
   192.168.10.50:49234 → 185.220.101.45:443
   - TLS encrypted (no payload visible)
   - Beacon pattern: Every 60 seconds
   - Duration: 5 hours

2. Internal Port Scan (11:00:00 - 11:10:00)
   192.168.10.50 → 192.168.10.0/24
   - TCP SYN scan (Nmap signature)
   - Ports scanned: 21, 22, 80, 443, 3306, 3389
   - 15 live hosts discovered

3. Web Application Attack (12:00:00)
   192.168.10.50 → 192.168.10.100:80
   - SQL injection payloads detected
   - URL: /admin/login.php?user=' OR 1=1--
   - Response: HTTP 200 (success)

4. Database Access (13:00:00)
   192.168.10.100 → 192.168.10.200:3306
   - MySQL authentication successful
   - Queries: SELECT * FROM customers
   - 45,000 rows retrieved

5. Data Exfiltration (14:00:00)
   192.168.10.50 → 185.220.101.45:443
   - Large outbound transfer: 43.5 MB
   - File signature: ZIP archive (password protected)
   - Customer database (likely)

DNS QUERIES:
09:31:00 - malicious-infra.ru → 185.220.101.45
11:15:00 - update-service.secure-corp-portal.com → 185.220.101.45