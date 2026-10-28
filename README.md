# 🛡️ TryHackMe Journey — SOC Level 1

> Hands-on log of my path to becoming a **SOC (Security Operations Center) Analyst**: every room, when I did it, what I learned and where I tripped.

![Path](https://img.shields.io/badge/path-SOC%20Level%201-blue)
![Progress](https://img.shields.io/badge/progress-~20%25-orange)
![Started](https://img.shields.io/badge/started-Oct%2020%2C%202026-lightgrey)

**Marco Rodrigues** · Cybersecurity student @ PUCPR (Brazil) · CompTIA Security+ (SY0-701)
TryHackMe profile: [tryhackme.com/p/marcorodrigues](https://tryhackme.com/p/marcorodrigues)

---

## TL;DR

- 🎯 **Goal:** SOC L1 (Level 1) Analyst as entry point → AppSec (Application Security) / DevSecOps (Development, Security and Operations).
- 📈 **Progress:** 17 rooms in 4 modules (Blue Team Introduction, SOC Team Internals, Core SOC Solutions, part of Cyber Defence Frameworks).
- 🧪 **Applying it:** building **snowSIEM**, a homelab SIEM (Security Information and Event Management) on the Elastic Stack, with my own detections validated against simulated attacks.

---

## 📅 Room log

| Date | Module | Room | Key takeaway |
|---|---|---|---|
| Oct 20 | Blue Team Introduction | Junior Security Analyst Intro | Daily routine and responsibilities of an L1 analyst |
| Oct 20 | Blue Team Introduction | SOC Role in Blue Team | SOC tiers (L1/L2/L3) and how the career progresses |
| Oct 20 | Blue Team Introduction | Humans as Attack Vectors | Phishing and social engineering as the main entry points |
| Oct 20 | Blue Team Introduction | Systems as Attack Vectors | How vulnerable and misconfigured systems get exploited |
| Oct 20 | SOC Team Internals | SOC L1 Alert Triage | Systematic triage: TP/FP (True/False Positive), severity, context |
| Oct 20 | SOC Team Internals | SOC L1 Alert Reporting | When and how to escalate; writing clear incident reports |
| Oct 20 | SOC Team Internals | SOC Workbooks and Lookups | Workbooks and asset/identity lookups to speed up triage |
| Oct 20 | SOC Team Internals | SOC Metrics and Objectives | MTTD (Mean Time to Detect), MTTR (Mean Time to Respond), FP rate |
| Oct 20 | SOC Team Internals | Introduction to Phishing *(SOC Simulator)* | First hands-on triage in a simulated SOC queue |
| Oct 21 | Core SOC Solutions | Introduction to EDR | EDR (Endpoint Detection and Response): endpoint telemetry and response |
| Oct 21 | Core SOC Solutions | Introduction to SIEM | Log collection, correlation and alerting |
| Oct 21 | Core SOC Solutions | Splunk: The Basics | Log investigations with SPL (Search Processing Language) |
| Oct 22 | Core SOC Solutions | Elastic Stack: The Basics | KQL (Kibana Query Language), Discover, Lens visualizations |
| Oct 22 | Core SOC Solutions | Introduction to SOAR | SOAR (Security Orchestration, Automation, and Response) playbooks; automation vs. human judgment |
| Oct 24 | Cyber Defence Frameworks | Pyramid of Pain | IOC (Indicator of Compromise) value; Emotet User-Agent as a network artifact |
| Oct 24 | Cyber Defence Frameworks | Cyber Kill Chain | Mapping the 2013 Target breach to the 7 stages |
| Oct 27 | Cyber Defence Frameworks | Unified Kill Chain | 18 phases in 3 stages (In, Through, Out); phishing as initial foothold; data leaks hit Confidentiality in the CIA triad |
| ⏳ | Cyber Defence Frameworks | MITRE | — |
| ⏳ | Cyber Defence Frameworks | Summit | — |
| ⏳ | Cyber Defence Frameworks | Eviction | — |

---

## 🧠 Lessons learned the hard way

**1. Check the timezone before investigating.** *(Elastic Stack: The Basics)*
Kibana shows timestamps in the browser's timezone (UTC−3 for me), while the dataset was built in UTC (Coordinated Universal Time). The "spike on Jan 11" was invisible on my screen because it had shifted outside the window. Fix: `Stack Management → Advanced Settings → dateFormat:tz = UTC`. In a real incident this breaks the whole timeline.

**2. `Unique count` ≠ `Count`.** *(Elastic Stack: The Basics)*
Unique count answers *"how many different actors?"*; count answers *"how much noise?"*. To find who caused a spike: **Count of records + break down by `Source_ip`**.

**3. A bare word in KQL is a free-text search.** *(Elastic Stack: The Basics)*
Typing `Source_ip` alone in the search bar searches for that literal text and returns zero hits. The right form is `field : "value"`, e.g.
```
Source_Country : "United States" and (UserName : "James" or UserName : "Albert")
```

**4. Trust the data, not the walkthrough.** *(Elastic Stack: The Basics, Cyber Kill Chain)*
Answers copied from public write-ups were wrong for my lab instance more than once. The logs in front of you are the source of truth.

**5. Terminated users still connecting = instant ticket.** *(Elastic Stack: The Basics)*
A VPN (Virtual Private Network) login from a user terminated weeks earlier means either the account was never disabled or the credentials are being abused. First action: disable the account, then investigate what the session did.

**6. Malware leaves fingerprints in headers.** *(Pyramid of Pain)*
Emotet often uses outdated, hardcoded User-Agents (e.g. MSIE 7.0 on Windows 7). Easy IOC to hunt and a cheap detection rule.

**7. Spelling counts in the answer box.** *(Unified Kill Chain)*
Right concept, wrong spelling ("confidenciality") still fails the check, and too many retries trigger a rate limit. Slow down and double-check English terms.

---

## 🔨 Where I apply this

- **snowSIEM** *(in progress)* — homelab SIEM on ELK (Elasticsearch, Logstash, Kibana): custom Logstash pipelines, ECS (Elastic Common Schema) normalization, detections mapped to MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge), validated with Atomic Red Team.
- Detection ideas already born from these rooms:
  - [ ] VPN login by a terminated / disabled user
  - [ ] Outdated browser User-Agent leaving the internal network
  - [ ] Single source IP spiking VPN connections

---

## 🗺️ Roadmap

- [x] Blue Team Introduction
- [x] SOC Team Internals
- [x] Core SOC Solutions
- [ ] Cyber Defence Frameworks *(in progress)*
- [ ] Phishing Analysis
- [ ] Network Traffic Analysis
- [ ] Network Security Monitoring
- [ ] Web Security Monitoring
- [ ] Windows Security Monitoring
- [ ] Linux Security Monitoring
- [ ] Malware Concepts for SOC
- [ ] Threat Analysis Tools
- [ ] SIEM Triage for SOC
- [ ] Capstone Challenges
- [ ] 🎓 SOC Level 1 certificate

---

## 📖 Glossary

| Acronym | Expansion |
|---|---|
| SOC | Security Operations Center |
| SIEM | Security Information and Event Management |
| EDR | Endpoint Detection and Response |
| SOAR | Security Orchestration, Automation, and Response |
| IOC | Indicator of Compromise |
| KQL | Kibana Query Language |
| SPL | Search Processing Language |
| ELK | Elasticsearch, Logstash, Kibana |
| ECS | Elastic Common Schema |
| MITRE ATT&CK | Adversarial Tactics, Techniques, and Common Knowledge |
| TP / FP | True Positive / False Positive |
| MTTD / MTTR | Mean Time to Detect / Mean Time to Respond |
| VPN | Virtual Private Network |
| UTC | Coordinated Universal Time |
| UKC | Unified Kill Chain |
| CIA | Confidentiality, Integrity, Availability |
| PII | Personally Identifiable Information |

---

> ⚠️ No flags or full answers are published here — only concepts and my own notes, in line with TryHackMe's rules.
