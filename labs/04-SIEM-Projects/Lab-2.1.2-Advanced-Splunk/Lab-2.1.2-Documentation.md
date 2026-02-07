#  LAB 2.1.2: Advanced Splunk - Detection Engineering & Threat Hunting

## PROJECT OVERVIEW

Today, elevating your Splunk level from basic operator to detection engineer. I'll learn advanced SPL techniques, create complex correlations, implement Sigma rules, and conduct your first proactive threat hunting session.

**Real-World Application:**

Detection engineers create the rules that SOC analysts use. Threat hunters look for threats that automatic alerts do not detect. Today you will learn about both roles.

## LAB ENVIRONMENT

- Splunk: Running from Day 8
- Data: Existing indexes + new hunt datasets

## PREREQUISITES

- Lab 2.1.1 completed 
- Splunk running with data indexed
- Understanding of basic SPL
- MITRE ATT&CK familiarity


## STEP BY STEP - LAB 2.1.2

**STEP 2.1.2.1: Advanced SPL Techniques**

**Technique 1:** ***Regular Expressions (regex)***
- Use Case: Extract specific patterns from unstructured logs
- Example: Extract IP addresses from messages

```spl
index="windows_security"
| rex field=Message "(?<extracted_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
| table _time, Message, extracted_ip
```

