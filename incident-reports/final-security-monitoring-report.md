# Security Monitoring & Detection Engineering Report
**Project:** Wazuh SIEM Local Laboratory Implementation  
**Author / Engineer:** Vincencio Gabriel Frederic Sanjaya  
**Target Environment:** Docker Single-Node (macOS) | Wazuh SIEM v4.14.7  

---

## 1. Executive Summary
This project demonstrates the deployment, configuration, and operational workflow of a Security Information and Event Management (SIEM) environment using Wazuh. The primary objective was to establish a local security monitoring laboratory to collect telemetry, analyze security events, implement custom detection logic, map threats to the **MITRE ATT&CK®** framework, and simulate a foundational SOC incident investigation workflow.

---

## 2. Environment Architecture & Scope
* **SIEM Platform:** Wazuh 4.14.7 (Manager, Indexer, Dashboard)
* **Deployment Method:** Docker Single-Node Containerization
* **Host Operating System:** macOS (Local Laboratory Testing)
* **Assessment Scope:**
  * File Integrity Monitoring (FIM)
  * Authentication Monitoring & SSH Log Parsing
  * Event Correlation & Custom Detection Engineering
  * MITRE ATT&CK Mapping & Incident Triage

---

## 3. Detection Scenarios & Implementation

### 3.1. Scenario 1: File Integrity Monitoring (FIM)
* **Objective:** Detect unauthorized or unexpected file lifecycle changes (creation, modification, deletion) within a sensitive directory (`/tmp/wazuh-lab`).
* **Triggered Rules & Results:**
  * **File Added:** Rule ID `554` (Level 5) — *File added to the system.*
  * **File Modified:** Rule ID `550` (Level 7) — *Integrity checksum changed.*
  * **File Deleted:** Rule ID `553` (Level 7) — *File deleted.*
* **Security Implication:** FIM is critical for detecting persistence mechanisms, configuration tampering, and unauthorized artifact placement by adversaries.
* **Evidence:** `screenshots/fim-alerts.png`

### 3.2. Scenario 2: SSH Failed Authentication Detection
* **Objective:** Parse custom syslog sources (`/tmp/wazuh-auth.log`) to detect brute-force precursors and invalid user login attempts.
* **Log Sample:** 
  > `Sep 28 09:10:01 lab sshd[2001]: Failed password for invalid user admin from 192.168.56.20 port 51001 ssh2`
* **Parsed Attributes:** Source IP: `192.168.56.20` | Target User: `admin` | Decoder: `sshd`
* **Base Alert:** Rule ID `5710` (Level 5) — *Attempt to login using a non-existent user.*

### 3.3. Scenario 3: Custom SSH Brute-Force Correlation Rule
* **Objective:** Reduce alert fatigue by correlating individual low-severity authentication failures into a single high-severity incident indicator based on frequency and timeframe.
* **Correlation Logic:** 5 failed authentication events (`sid: 5710`) from the same source IP within a 60-second window.
* **Custom Rule Configuration (XML Syntax Reference):**
  - Group Name: `custom_ssh,authentication_failed`
  - Rule ID: `100100` | Level: `10` | Frequency: `5` | Timeframe: `60s`
  - Matching Condition: If matched SID `5710` with `<same_srcip />`
  - Description: `Possible SSH brute-force attack: multiple failed logins from same source IP`
  - MITRE ATT&CK ID: `T1110`
* **Detection Result:** Rule ID `100100` (Level 10) triggered successfully upon threshold breach.
* **Evidence:** `screenshots/failed-login-alerts.png`, `screenshots/custom-bruteforce-rule.png`

---

## 4. Incident Analysis & Threat Intelligence Mapping

* **Classification:** Authentication Attack / Brute-Force
* **MITRE ATT&CK Technique:** [T1110 - Brute Force](https://attack.mitre.org/techniques/T1110/)
* **Severity Level:** High (Level 10)
* **Investigation Workflow:**
  1. Triage triggered custom rule `100100` and associated base events (`5710`).
  2. Extract indicators of compromise (IoCs): Source IP (`192.168.56.20`) and target account (`admin`).
  3. Validate event frequency and inspect timeline continuity within the 60-second sliding window.
  4. Check for subsequent successful authentication events following the brute-force phase.
* **Remediation Recommendations:**
  * Implement source IP blocking or rate-limiting at the firewall/perimeter.
  * Enforce SSH key-based authentication and disable password-based logins.
  * Mandate Multi-Factor Authentication (MFA) where applicable.
  * Audit targeted accounts for potential credential compromise or unauthorized access.

---

## 5. Key Competencies Demonstrated
* SIEM Architecture & Docker Deployment
* Log Collection & Advanced Parsing (Syslog / Auth Logs)
* File Integrity Monitoring (FIM)
* Detection Engineering (Custom Rules & Threshold Correlation)
* Threat Hunting & Incident Triage
* MITRE ATT&CK Framework Mapping
* Technical Documentation & Reporting

---

## 6. Limitations & Scope Constraints
* **Environment Scale:** Conducted in an isolated single-node Docker laboratory environment without production endpoint agents.
* **Telemetry Simulation:** Authentication logs and events were artificially simulated for validation purposes.

---

## 7. Conclusion
The Wazuh SIEM detection laboratory successfully validated end-to-end security monitoring capabilities, ranging from log ingestion and rule parsing to advanced event correlation and threat mapping. The custom correlation rule effectively elevated discrete low-severity events into an actionable high-priority security incident, demonstrating foundational Detection Engineering and Security Operations proficiency.