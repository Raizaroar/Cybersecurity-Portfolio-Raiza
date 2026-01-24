
```
Event ID: 4625 (Failed Logon)
Time: 2025-01-22 09:15:00
Account: admin
Source IP: 192.168.10.50
Reason: Bad password
---
Event ID: 4624 (Successful Logon)
Time: 2025-01-22 09:30:00
Account: jsmith (Finance Dept)
Source IP: 192.168.10.50
Logon Type: 2 (Interactive)
---
Event ID: 4688 (Process Creation)
Time: 2025-01-22 09:31:00
Process: C:\Users\jsmith\Downloads\Benefits_Enrollment_2025.xlsx.exe
Parent: explorer.exe
User: jsmith
---
Event ID: 4688 (Process Creation)
Time: 2025-01-22 09:31:30
Process: C:\Users\Public\svchost.exe
Parent: Benefits_Enrollment_2025.xlsx.exe
User: jsmith
---
Event ID: 5156 (Network Connection)
Time: 2025-01-22 09:32:00
Process: C:\Users\Public\svchost.exe
Direction: Outbound
Destination IP: 185.220.101.45
Destination Port: 443
Protocol: TCP
---
Event ID: 4688 (Process Creation)
Time: 2025-01-22 11:00:00
Process: C:\Windows\System32\net.exe
CommandLine: net view
User: jsmith
---
Event ID: 5156 (Network Connection)
Time: 2025-01-22 11:05:00
Process: powershell.exe
Direction: Outbound
Destination IP: 192.168.10.100 (WEB-APP-01)
Destination Port: 80
---
Event ID: 4663 (File Access)
Time: 2025-01-22 13:30:00
File: \\DB-SQL-01\SharedData\customer_database.xlsx
User: jsmith
Access: Read
---
Event ID: 5156 (Network Connection)
Time: 2025-01-22 14:00:00
Process: powershell.exe
Direction: Outbound
Destination IP: 185.220.101.45
Destination Port: 443
Bytes Sent: 45,678,912 (43.5 MB)