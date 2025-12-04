# Network Traffic Analysis with Wireshark

## Project Overview

This project demonstrates fundamental network traffic analysis skills using Wireshark. The analysis focuses on identifying common protocols, understanding packet structures, and detecting potential security issues in captured network traffic.

Duration: 2-3 hours
Difficulty: Entry Level
Platform: Kali Linux 2025.2-virtualbox-amd64

Tools Used:

- Wireshark (GUI analysis)
- tcpdump (packet capture)
- tshark (CLI analysis)
- capinfos (statistics)
- Linux command line utilities

Objectives

1. Capture live network traffic using Wireshark
2. Analyze common protocols (HTTP, DNS, TCP, ICMP)
3. Identify normal vs suspicious network behavior
4. Extract relevant information from packet captures
5. Document findings in a professional format

-**Lab Environment**

 System Information

OS: Kali Linux 2025.2-virtualbox-amd64

Wireshark Version: 4.x (pre-installed in Kali)

Network Interface: eth0 / wlan0

Capture Duration: 5-10 minutes per scenario

-**Prerequisites**

Kali Linux VM or bare metal installation

Root/sudo privileges

Basic understanding of TCP/IP and Linux CLI

Network connectivity for packet capture

## Wireshark Traffic Analysis - Detailed Report

Analyst: Raiza Rosas Aguilar
Date: 15-11-25
Duration: 10 minutes
Total Packets: [número]

## Executive Summary

This report documents the analysis of network traffic captured during a 10-minute session. The analysis focused on identifying common protocols, understanding data flow, and detecting potential security concerns.
Key Findings:

- HTTP traffic exposes unencrypted data including URLs and cookies
- DNS queries reveal browsing patterns
- TCP connections established successfully with proper handshakes
- No malicious activity detected during capture period

 Methodology
Capture Setup

Platform: Kali Linux 2025.2-virtualbox-amd64

Tool: Wireshark 4.x / tcpdump / tshark

Interface: eth0 (Ethernet) / wlan0 (Wireless)

## Capture Command

```bash
sudo tcpdump -i eth0 -w http-traffic-analysis.pcap
```

Filter: None (captured all traffic)
File Size: [X] MB
Location: captures/http-traffic-analysis.pcap

## Analysis Approach

Initial packet review using Wireshark GUI

CLI analysis with tshark for automation

Protocol-specific filtering (HTTP, DNS, TCP)

Stream following for context

Statistical analysis with capinfos

Security assessment

## Traffic Analysis Results

- **HTTP Traffic Analysis**
 Filter Used: http
Observations:

Captured 1967 HTTP Packets

All requests to http://neverssl.com
User-Agent: Mozilla/5.0 (identifying browser)
HTTP Methods observed: GET, POST
