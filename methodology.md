Wazuh SIEM Detection Lab Methodology
Objective
The objective of this project was to build a local SIEM environment using Wazuh and simulate a basic SOC workflow.
The project focused on log collection, event detection, alert analysis, custom correlation rules, and incident investigation.
Environment
Platform:
macOS
Deployment:
Docker single-node
SIEM:
Wazuh 4.14.7
Main Components:
Wazuh Manager
Wazuh Indexer
Wazuh Dashboard
Testing Scope
All activities were performed inside a controlled local environment.
The testing focused on:
File Integrity Monitoring
SSH authentication monitoring
Failed login detection
Custom rule creation
Event correlation
MITRE ATT&CK mapping
Alert investigation
Testing Methodology
1. SIEM Deployment
Wazuh was deployed using Docker in a single-node configuration.
The environment included:
Wazuh Manager
Wazuh Indexer
Wazuh Dashboard
The dashboard was used to review and investigate generated security events.
2. File Integrity Monitoring
A dedicated directory was created:
/tmp/wazuh-lab
Wazuh File Integrity Monitoring was configured to monitor this directory in real time.
Three file activities were generated:
File creation
File modification
File deletion
The resulting alerts were reviewed in the File Integrity Monitoring dashboard.
3. Authentication Log Collection
A custom authentication log file was created:
/tmp/wazuh-auth.log
The log file was configured as a syslog source in Wazuh.
Simulated SSH authentication failures were written to the file.
Example:
Sep 28 09:10:01 lab sshd[2001]: Failed password for invalid user admin from 192.168.56.20 port 51001 ssh2
4. Event Decoding
Wazuh processed the SSH log and extracted relevant information such as:
Source IP
Username
SSH decoder
Original log message
The event triggered the built-in SSH rule:
Rule ID:
5710
Rule Level:
5
5. Custom Detection Engineering
A custom correlation rule was created to detect repeated failed login attempts.
The rule used:
Base Rule:
5710
Frequency:
5 events
Timeframe:
60 seconds
Condition:
Same source IP
Custom Rule ID:
100100
Rule Level:
10
MITRE ATT&CK:
T1110 Brute Force
6. Detection Validation
Multiple SSH login failures were generated from the same simulated source IP.
Wazuh first generated individual Rule 5710 alerts.
After the configured threshold was reached, the custom Rule 100100 generated a higher-severity alert.
7. Alert Investigation
Alerts were reviewed using Wazuh Threat Hunting.
The investigation focused on:
Source IP
Target username
Rule ID
Rule level
Timestamp
Original log
Related events
Correlation results
8. Evidence Collection
Screenshots were collected for:
File Integrity Monitoring alerts
Failed login alerts
Custom brute-force correlation
Evidence was stored in:
screenshots/
9. Incident Analysis
The detected activity was analyzed using a basic SOC workflow:
Event Detection
Alert Review
Context Analysis
Correlation
Impact Assessment
Response Recommendation
10. Documentation
Each detection scenario was documented separately.
The final incident report summarizes:
Detection logic
Triggered alerts
Security analysis
MITRE ATT&CK mapping
Recommended remediation
Limitations
The environment was a local laboratory.
SSH events were simulated.
No production endpoints were monitored.
The project focused on basic SIEM and detection engineering concepts.
remediation.md
Wazuh SIEM Remediation and Response Guide