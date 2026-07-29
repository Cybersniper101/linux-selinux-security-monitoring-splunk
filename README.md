# linux-selinux-security-monitoring-splunk
Linux security monitoring project using SELinux, auditd, and Splunk to detect, analyze, and visualize unauthorized access attempts.


# Linux Security Monitoring with SELinux & Splunk

![Kali Linux](https://img.shields.io/badge/Linux-Kali%20Linux-blue?style=for-the-badge&logo=kalilinux)
![SELinux](https://img.shields.io/badge/SELinux-Enforcing-success?style=for-the-badge)
![Splunk](https://img.shields.io/badge/Splunk-Enterprise-black?style=for-the-badge&logo=splunk)
![SIEM](https://img.shields.io/badge/SIEM-Security%20Monitoring-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

# Linux Security Monitoring with SELinux & Splunk

## Project Overview

Modern organizations generate thousands of security events every day. Detecting and investigating these events quickly is critical to preventing unauthorized access and minimizing security risks.

This project demonstrates how **SELinux (Security-Enhanced Linux)** and **Splunk Enterprise** can be integrated to monitor, collect, analyze, and visualize Linux security events.

Multiple unauthorized access attempts were intentionally simulated to trigger **SELinux Access Vector Cache (AVC) denial events**. These events were forwarded to **Splunk Enterprise**, where they were investigated using SPL (Search Processing Language) and visualized through interactive dashboards.

This project demonstrates practical experience in Linux security monitoring, SIEM administration, log analysis, and incident investigation.

---

# Objectives

The primary objectives of this project were to:

- Configure SELinux in Enforcing mode
- Generate multiple SELinux denial events
- Configure Splunk to ingest Linux audit logs
- Investigate SELinux security events using SPL
- Identify denied processes
- Identify affected files, directories, and ports
- Determine the number of denial events
- Identify event timestamps
- Build a security monitoring dashboard
- Document findings and security observations

---

# Skills Demonstrated

- Linux Administration
- Linux Security Monitoring
- SELinux Policy Enforcement
- Mandatory Access Control (MAC)
- auditd Configuration
- Splunk Enterprise Administration
- Log Collection & Analysis
- Search Processing Language (SPL)
- SIEM Operations
- Security Event Investigation
- Dashboard Development
- Incident Analysis
- Technical Documentation

---

# Lab Environment

| Component | Technology |
|-----------|------------|
| Operating System | Kali Linux 2026.1 |
| Security Framework | SELinux |
| Logging Service | auditd |
| SIEM | Splunk Enterprise |
| Log Source | /var/log/audit/audit.log |
| Virtualization | Oracle VirtualBox |

---

# Tools Used

- Kali Linux
- SELinux
- auditd
- Splunk Enterprise
- Splunk Universal Forwarder
- Bash
- Linux Command Line
- Oracle VirtualBox

---

# Project Architecture

```
                 +----------------------+
                 |     Kali Linux VM    |
                 +----------------------+
                            |
                     SELinux (Enforcing)
                            |
                  Generates AVC Denials
                            |
                         audit.log
                            |
                 Splunk Universal Forwarder
                            |
                     Splunk Enterprise
                            |
          +-----------------+-----------------+
          |                 |                 |
     SPL Searches      Dashboards      Investigations
```

---

# Project Workflow

```
Configure SELinux

↓

Generate Security Events

↓

Store Events in audit.log

↓

Forward Logs to Splunk

↓

Search with SPL

↓

Investigate Events

↓

Build Dashboard

↓

Document Findings
```

---

# Implementation

## Step 1 — Configure SELinux

SELinux was configured in **Enforcing Mode** to ensure unauthorized operations were blocked and logged.

Command:

```bash
getenforce
```

Output

```
Enforcing
```

Screenshot:
![SELinux Status](screenshots/gentenforce.jpg)
```

## Step 2 — Generate SELinux Denial Events

Three different security policy violations were intentionally created.

### Scenario 1 — Unauthorized File Access

A process attempted to access a protected file.

Expected Result

SELinux denied the operation and generated an AVC denial event.

---

### Scenario 2 — Web Server Access to Restricted File

The web server attempted to access a restricted file outside its permitted SELinux context.

Expected Result

SELinux blocked the request.

---

### Scenario 3 — Unauthorized Port Usage

An application attempted to bind to a port that was not permitted by the current SELinux policy.

Expected Result

SELinux denied the request and generated an audit event.

---

# Log Collection

Linux audit logs were monitored and forwarded into Splunk Enterprise for centralized analysis.

Log source:

```
/var/log/audit/audit.log
```

Splunk successfully indexed all generated AVC denial events.

---

# SPL Queries

## Total SELinux Denials

```spl
index=linux_audit AVC
| stats count
```

---

## Top Denied Processes

```spl
index=linux_audit AVC
| top comm
```

---

## Top Denied Resources

```spl
index=linux_audit AVC
| top tcontext
```

---

## Denial Timeline

```spl
index=linux_audit AVC
| timechart count
```

---

## Event Details

```spl
index=linux_audit AVC
| table _time comm exe name tcontext
```

---

# Dashboard

A custom Splunk dashboard was created to visualize Linux security activity.

Dashboard Panels

### Total SELinux Denials

Displays the total number of AVC denial events detected.

---

### Top Denied Processes

Shows which Linux processes generated the highest number of SELinux denials.

---

### Top Denied Resources

Displays the files, directories, or ports most frequently protected by SELinux.

---

### Denial Timeline

Shows when denial events occurred throughout the monitoring period.

---

# Investigation Results

The investigation successfully identified:

- The process responsible for each denial event
- The affected file or resource
- Event timestamps
- Total denial events generated
- Frequently denied processes
- Frequently targeted resources

---

# Screenshots

## SELinux Status

![SELinux Status](screenshots/gentenforce.jpg)

---

## Audit Log Events

![Audit Logs](screenshots/splunk_audit.log.png)

---

## Splunk Search Results

![Splunk Search](screenshots/splunk.denial_command.jpg)

---

## Total SELinux Denials

![Total Denials](screenshots/splunk_errors.jpg)

---

## Dashboard Overview

![Dashboard](screenshots/splunk_dashboard.jpg)
![Dashboard](screenshots/splunk_dashboard_2.jpg)
![Dashboard](screenshots/splunk_dashboard_3.jpg)

---

# Key Findings

- SELinux successfully prevented unauthorized system activity.
- Linux audit logs provided detailed forensic evidence.
- Splunk centralized Linux security logs for efficient investigation.
- SPL simplified security event analysis.
- Dashboards improved visibility into Linux security events.
- Combining SELinux with Splunk creates an effective host-based security monitoring solution.

---

# Lessons Learned

Through this project I gained practical experience in:

- Linux Security Monitoring
- SELinux Policy Enforcement
- Mandatory Access Control (MAC)
- Linux Audit Logging
- Splunk Administration
- Security Event Investigation
- SPL Query Development
- Dashboard Design
- Threat Detection
- Security Documentation

---

# Future Improvements

- Configure real-time Splunk alerts
- Integrate email notifications
- Monitor multiple Linux systems
- Build incident response playbooks
- Integrate Sysmon for Linux
- Map events to the MITRE ATT&CK Framework
- Automate reporting using Splunk scheduled reports

---

# Repository Structure

```
linux-security-monitoring-selinux-splunk/

│
├── README.md
├── LICENSE
├── .gitignore
│
├── screenshots/
│   ├── selinux-status.jpg
│   ├── audit-log.jpg
│   ├── splunk_denial_results.jpg
│   ├── splunk_logs_errors.jpg
│   ├── search-results.jpg
│   └── dashboard 1.jpg
│   ├── dashboard 2.jpg
│   ├── dashboard 3.jpg

│
├── configs/
│   ├── inputs.conf
│   ├── props.conf
│   └── auditd.conf
│
├── logs/
│   └── /var/log/audit/audit.log
│
├── queries/
│   ├── total_denials.spl
│   ├── top_processes.spl
│   ├── top_resources.spl
│   ├── event_details.spl
│   └── denial_timeline.spl
│
└── docs/
    ├── WALKTHROUGH.md
    ├── ATTACK_SCENARIOS.md
    └── PROJECT_REPORT.pdf
```

---

# Key Cybersecurity Concepts

- Linux Security
- SELinux
- Mandatory Access Control
- SIEM
- Security Monitoring
- Log Analysis
- Threat Detection
- Incident Investigation
- Blue Team Operations
- SOC Monitoring
- Detection Engineering

---

# References

- SELinux Documentation
- Splunk Enterprise Documentation
- Linux auditd Documentation

---

# Author

## Olabode Williams (Cyber Sniper)

**Cybersecurity Analyst | Penetration Tester | Security Researcher**

I am passionate about Offensive Security, Detection Engineering, Linux Security, SIEM, Threat Hunting, and helping organizations improve their cybersecurity posture through practical security testing and monitoring.

📧 Email: *Cybersniper7016@gmail.com*

🔗 LinkedIn: *www.linkedin.com/in/olabode-williams-*

🐙 GitHub: *Cybersniper101*
