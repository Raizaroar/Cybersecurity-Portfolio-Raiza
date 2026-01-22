# Phishing Analysis: Fake Microsoft Account Suspension

**Date:** 20-01-2025  
**Analyst:** Raiza  
**Sample:** Microsoft account phishing  
**Verdict:** CONFIRMED PHISHING  

---

## Email Metadata

**From:** Microsoft Security Team <no-reply@microsoft-security.com>  
**To:** victim@company.com  
**Subject:** [URGENT] Your Microsoft account will be suspended  
**Date:** Mon, 20 Jan 2025 10:30:00 +0000  

---

## Analysis Summary

**Phishing Type:** Credential Harvesting  
**Target Brand:** Microsoft  
**Attack Vector:** Email with malicious link  
**Sophistication:** Medium (professional appearance, modern techniques)  

---

## Red Flags Identified

### Email Headers (CRITICAL)
- SPF: FAIL (unauthorized sender)
- DKIM: FAIL (invalid signature)
- DMARC: FAIL (policy violation)
- Return-Path mismatch (attacker@malicious-server.ru)
- Originating IP: 203.0.113.50 (blacklisted, Russia)

### Domain Analysis
- Typosquatting: microsoft-security.com vs. microsoft.com
- Reply-To different domain: fake-microsoft.net
- Malicious domain registered 2 days ago

### Content Analysis
- Urgency tactics ("24 hours", "suspended")
- Fear tactics (account closure threat)
- Generic greeting (not personalized)
- Suspicious link (different domain)

### URL Analysis
- Phishing site: malicious-site.com
- Fake login page detected
- Credential harvesting form
- Domain: 2 days old (newly registered)

---

## IOCs for Blocking

**Domains:**
- microsoft-security.com
- fake-microsoft.net
- malicious-site.com
- microsoft-secure-login.malicious-site.com

**IPs:**
- 203.0.113.50 (sender)
- 198.51.100.25 (phishing site)

**Email Addresses:**
- attacker@malicious-server.ru
- support@fake-microsoft.net
- no-reply@microsoft-security.com

---

## MITRE ATT&CK Mapping

- **T1566.002:** Phishing: Spearphishing Link
- **T1598.003:** Phishing for Information: Spearphishing Link
- **T1586.002:** Compromise Accounts: Email Accounts

---

## Recommended Actions

**Immediate:**
- [ ] Block all IOCs at email gateway
- [ ] Add domains to DNS blacklist
- [ ] Alert users about this phishing campaign
- [ ] Search mailboxes for similar emails

**Short-Term:**
- [ ] Implement DMARC policy for company domain
- [ ] Train users on phishing recognition
- [ ] Deploy anti-phishing browser extensions

**Long-Term:**
- [ ] Implement email authentication (SPF, DKIM, DMARC)
- [ ] Regular phishing simulations
- [ ] Advanced email security gateway