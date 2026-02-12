# DAY 11 - MORE DETECTION: Advanced Port Scanning Detection & Behavioral Analysis

## Project Overview

This lab expands on the detection capabilities of Day 10, focusing on advanced port scan evasion techniques and behavioral analysis to identify more sophisticated attackers who attempt to remain under the radar using techniques such as slow scans, fragmented packets, and randomized timing.

## Lab Environment

**Setup Required:**

- Attacker Machine: Kali Linux 
- Target Machine: Ubuntu Server
- Monitoring Machine: Security Onion or Ubuntu with Zeek + Suricata

**Network Configuration:**

- Network: 192.168.100.0/24
- Attacker: 192.168.100.10
- Target: 192.168.100.50
- SOC Workstation: 192.168.100.100

## Prerequisites

- Lab Day 10 completed
- Basic knowledge of TCP/IP
- Wireshark installed
- Access to virtual machines
- Python 3.8+ installed
- Active GitHub account

# Configuración actual del laboratorio SOC

```mermaid
graph TD
    Internet --> LabHackingNAT
    LabHackingNAT --> Kali[Kali Linux (Attacker) - 10.0.2.5]
    LabHackingNAT --> Meta[Metasploitable2 (Victim) - 10.0.2.4]
    LabHackingNAT --> Ubuntu[ Ubuntu SOC (Defender) - 10.0.2.7]

    Kali --> Meta
    Ubuntu --> Meta
