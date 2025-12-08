# Basic SQLi Scanner with Vulnerability Detection

## Project Overview

Automated scanner for detecting SQL injection vulnerabilities through error-based detection.

Duration: 1-2 hours
Difficulty: Entry Level
Platform: Kali Linux 2025.2-virtualbox-amd64

Tool Used:

   - Python 3.11
   - ```Requests``` library
   - ```BeautifulSoup4``` library

Objective

1. Develop an automated scanner that identifies SQL injection points in web forms by analyzing HTTP responses.

2. Identify common error patterns in databases.

-**Lab Environment**


System Information

OS: Kali Linux 2025.2-virtualbox-amd64

DVWA (Damn Vulnerable Web Application) in Docker

Python 3.13.9

-**Prerequisites**

Kali Linux VM 

Root/sudo privileges

## Basic SQLi Scanner with Vulnerability Detection

Analyst: Raiza Rosas Aguilar
Date: 02-12-25
Duration: 30 minutes

## Executive Summary 

This project implements an entry-level SQL injection vulnerability scanner that tests web forms with basic payloads. The scanner identifies vulnerabilities by detecting typical SQL error messages (MySQL, PostgreSQL, MSSQL) and analyzes changes in the HTTP status code. It was successfully tested against DVWA, identifying three injection points in low and medium security modes.

METHODOLOGY
Machine Used: Kali Linux 2024.x (VM)
Test Environment: DVWA (Damn Vulnerable Web Application) in Docker


**Step 1**

Install Python dependencies

```bash
sudo apt install python3-pip python3-venv -y
```
[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA.png)


Create virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA01.png)

Install necessary libraries

```bash
pip install requests beautifulsoup4 colorama
```

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA02.png)

Why these commands?

-Virtual environment (venv): Isolates project dependencies to avoid conflicts with other Kali tools

-Colorama: Provides colored output in the terminal, improving readability for reports


**Step 2**

Installing DVWA (Test Environment )

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA03.png)

Download and set up DVWA 

```bash
sudo docker pull vulnerables/web-dvwa
```

```bash
sudo docker run -d -p 80:80 vulnerables/web-dvwa
```

```docker run``` = runs a container

```-d``` = “detached” (runs in the background, does not block the terminal)
```-p 80:80``` = maps port 80 of the container to port 80 of your Kali

```vulnerables/web-dvwa``` = name of the image to run

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA04.png)


Verified is running

```bash
sudo docker ps
```

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA05.png)

Why DVWA?

- It is a legal and controlled environment for testing.
- It simulates real vulnerabilities without ethical/legal risks.
- It is widely recognized in the cybersecurity industry.

**badges**

Username: admin
Password: password

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA06.png)


**Configure the database**

- I see a large button that says “Create/Reset Database.”
- Click on it.
- Wait 10 seconds.
- I am redirected to the login page again.
- Log in again with admin/password.

**Configure security level:**

- In the left menu, click on “DVWA Security.”
- Select “Low” from the dropdown menu.
- Click on “Submit.”

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA07.png)


**Open VS code**



**TOOLS USED AND JUSTIFICATION**

-*Requests Library*

Why: Handles HTTP requests in a Python-like and robust way.
Alternative not chosen: urllib (more complex syntax, less readable).


-*BeautifulSoup4*

Why: Efficient HTML parsing for response analysis.
Alternative not chosen: regex (prone to errors, difficult to maintain).


-*Colorama*

Why: Improves UX with colored output, standard in pentesting tools.
Alternative not chosen: Manual ANSI codes (not portable between systems).




***KEY TECHNICAL DECISIONS***

Why use error patterns instead of time analysis?

**Chosen**: Detection by SQL error patterns

**Discarded**: Time-based SQLi (delay analysis)

**Reason**: For an entry-level scanner, patterns are more reliable and faster. Time-based requires complex statistical analysis and can generate false positives on slow networks.

Why session requests?

-Maintains cookies between requests (necessary for DVWA)

-Reuses TCP connections (better performance)

-Essential for applications that require authentication

Key Findings:

100% detection on forms with weak validation
Average scan time: 2.3 seconds per endpoint
0 false positives in controlled tests

Conclusion

