Wazuh SIEM Detection Lab
Project Overview
This project demonstrates a local Security Information and Event Management lab using Wazuh.
The goal was to simulate a basic SOC workflow by collecting logs, detecting suspicious activity, analyzing alerts, and creating a custom correlation rule.
Environment
SIEM: Wazuh 4.14.7
Deployment: Docker single-node
Platform: macOS
Main Components:
Wazuh Manager
Wazuh Indexer
Wazuh Dashboard
Detection Scenarios
1. File Integrity Monitoring
Wazuh monitored:
/tmp/wazuh-lab
The following activities were detected:
File creation
File modification
File deletion
Detected Rules:
Rule 554
File added
Rule 550
Integrity checksum changed
Rule 553
File deleted
2. Failed SSH Login Detection
A custom authentication log source was configured at:
/tmp/wazuh-auth.log
Simulated SSH authentication failures were successfully decoded by Wazuh.
Detected Rule:
Rule ID: 5710
Rule Level: 5
Description:
sshd: Attempt to login using a non-existent user
Wazuh extracted information such as:
Source IP
Target username
SSH decoder
Original log message
3. Custom SSH Brute-Force Detection
A custom Wazuh correlation rule was created to detect repeated failed SSH login attempts.
Detection Logic:
5 failed authentication attempts
from the same source IP
within 60 seconds
Custom Rule:
Rule ID: 100100
Rule Level: 10
MITRE ATT&CK:
T1110
Brute Force
Detection Flow
Failed SSH Login
Rule 5710
Multiple Events Detected
Correlation Rule
Rule 100100
High-Severity Brute-Force Alert
Tools Used
Wazuh
Docker
Bash
Linux command line
Git
GitHub
Skills Demonstrated
SIEM deployment
Security monitoring
Log analysis
File Integrity Monitoring
Authentication monitoring
Alert triage
Custom Wazuh rules
Detection engineering
Event correlation
MITRE ATT&CK mapping
Incident analysis
Security documentation
Project Structure
wazuh-siem-detection-lab/
├── README.md
├── methodology.md
├── remediation.md
├── detections/
│   ├── 01-file-integrity-monitoring.md
│   └── 02-failed-login-detection.md
├── incident-reports/
│   └── final-security-monitoring-report.md
├── screenshots/
│   ├── fim-alerts.png
│   ├── failed-login-alerts.png
│   └── custom-bruteforce-rule.png
└── wazuh-docker/
Evidence
File Integrity Monitoring:
screenshots/fim-alerts.png
Failed Login Detection:
screenshots/failed-login-alerts.png
Custom Brute-Force Detection:
screenshots/custom-bruteforce-rule.png
Key Observation
Individual failed authentication attempts generated relatively low-level alerts.
By correlating multiple events from the same source IP, the custom detection rule generated a higher-severity alert.
This demonstrates how event correlation can help SOC analysts identify suspicious behavior more efficiently.
SOC Workflow
The project follows a basic security operations workflow:
Security Activity
Log Generation
Log Collection
Event Decoding
Rule Matching
Alert Generation
Event Correlation
Investigation
Remediation
Lessons Learned
This project helped demonstrate that effective SIEM monitoring depends on more than collecting logs.
Security events must be parsed correctly, correlated, and analyzed in context.
Custom rules can improve detection quality by turning multiple related low-level events into a single higher-priority alert.
Limitations
This project was performed in a local laboratory.
SSH authentication events were simulated.
The environment does not represent a full production SOC infrastructure.
Disclaimer
All activity was performed in a controlled local environment for educational and portfolio purposes.# wazuh-siem-detection-lab
