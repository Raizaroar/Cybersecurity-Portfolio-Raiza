# SIEM Dashboard - Detección SSH Brute Force

## Project Overview

This cybersecurity project simulates brute-force attacks targeting SSH services and leverages Splunk to detect and visualize malicious activity in real time. Using Kali Linux as the attacker and Ubuntu as the victim environment, I employed Hydra to generate failed login attempts and configured Splunk Universal Forwarder and Splunk Enterprise to collect and analyze log data.

Duration: 4 hours
Difficulty: Entry Level
Platform: Kali Linux, Ubuntu

Tools:

   -  Hydra

   - Splunk Universal Forwarder

   - Splunk Enterprise

Objective

1. Create a dashboard that detects and visualizes brute force attempts against SSH services in real time

-**Lab Enviroment**

Kali Linux 2025.2-virtualbox-amd64

Ubuntu AMD 24.04.3 TLS

Network Interface: eth0 / wlan0

-**Prerequistes**

Basic Python or Bash scripting

SSH Protocol Fundamentals

Splunk Components

Hydra Tool Basics

## SIEM Dashboard - Detección SSH Brute Force - Detailed Report

Analyst: Raiza Rosas Aguilar
Date: 30-11-25
Duration: 20 minutes
Total Logs: [110]

## Executive Summary

This project simulates SSH brute force attacks and configures Splunk to detect them automatically. I implement alerts based on thresholds for failed attempts and create visualizations that allow a SOC team to identify attack patterns. The result is a functional dashboard that displays attacking IPs, frequency of attempts, and attack timelines.

Methodology

Machine used: Kali Linux (attacker) + Ubuntu machine (simulated victim)

**Step 1** 

In the victim environment, I place this script.

```
sudo apt update
sudo apt install openssh-server -y
sudo systemctl start ssh
sudo systemctl enable ssh
```

I checked SSH is running

```
sudo systemctl status ssh
```


I Verificed SSH it's right

 ```
sudo systemctl status ssh
```

## poner ss aqui

Why these commands?

I need an active SSH service to attack in a controlled manner

```systemctl enable``` ensures that SSH starts automatically

**Step 2: Installing Splunk Universal Forwarder on the Victim**

Download forwarder 
```
wget -O splunkforwarder.tgz 'https://download.splunk.com/products/universalforwarder/releases/9.1.0/linux/splunkforwarder-9.1.0-linux-2.6-amd64.deb'
```

Install
```
sudo dpkg -i splunkforwarder-9.1.0-linux-2.6-amd64.deb
```

Configure to monitor authentication logs

```
sudo /opt/splunkforwarder/bin/splunk start --accept-license
sudo /opt/splunkforwarder/bin/splunk add forward-server [IP_DE_TU_SPLUNK]:9997
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log -index main
```

## poner sss aqui

***Why this path?***

SSH logs are stored in ```/var/log/auth.log.```

Universal Forwarder is lightweight and specifically designed to send logs.

**Port 9997** is the standard for receiving data in Splunk.

**Step 3: Attak Simulation** 

From Kali Linux
I created a field users commons

```
echo -e "root\nadmin\nuser\ntestuser" > users.txt
```

# Crea un archivo de contraseñas comunes
echo -e "password\n123456\nadmin\nletmein" > passwords.txt

# Ejecuta Hydra
hydra -L users.txt -P passwords.txt ssh://[IP_VICTIMA] -t 4 -V


## Summary 
