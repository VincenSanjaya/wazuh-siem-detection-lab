This document describes recommended response and remediation actions for the security events detected during the Wazuh SIEM Detection Lab.
1. File Integrity Monitoring
Detected Activities:
File creation
File modification
File deletion
Potential Risk
Unexpected file changes may indicate:
Unauthorized configuration changes
Malware activity
Persistence mechanisms
File tampering
Deletion of important files
Recommended Response
Identify the affected file.
Review the timestamp of the modification.
Identify the user or process responsible.
Compare the file with a known-good version.
Review nearby security events.
Restore the original file if required.
Recommended Remediation
Restrict write permissions on sensitive directories.
Apply least privilege.
Monitor critical files in real time.
Maintain backups of important files.
Use change management for production systems.
Investigate unexpected file deletions immediately.
2. Failed SSH Authentication
Detected Rule:
Rule ID:
5710
Description:
sshd: Attempt to login using a non-existent user
Potential Risk
Repeated authentication failures may indicate:
Password guessing
Brute-force attacks
Credential stuffing
Account enumeration
Unauthorized access attempts
Recommended Response
Identify the source IP.
Review targeted usernames.
Count failed authentication attempts.
Check whether a successful login followed the failures.
Review other events from the same IP.
Determine whether the activity is expected.
Recommended Remediation
Use strong passwords.
Disable unused accounts.
Prefer SSH key authentication.
Restrict SSH access.
Use multi-factor authentication where available.
Apply rate limiting.
Monitor repeated login failures.
3. SSH Brute-Force Detection
Custom Rule:
Rule ID:
100100
Rule Level:
10
MITRE ATT&CK:
T1110 Brute Force
Detection Condition:
5 failed SSH login attempts
from the same source IP
within 60 seconds
Potential Risk
Repeated authentication attempts from one source may indicate an active brute-force attack.
Recommended Response
Investigate the source IP.
Review all authentication attempts from that source.
Check whether any successful login occurred.
Review the affected account.
Search for additional suspicious activity.
Temporarily block the source IP if appropriate.
Escalate the alert if compromise is suspected.
Recommended Remediation
Enable authentication rate limiting.
Use SSH keys instead of password authentication.
Disable direct root login.
Use firewall rules to restrict SSH exposure.
Enable multi-factor authentication where possible.
Apply account lockout protections carefully.
Monitor authentication activity continuously.
4. Alert Correlation
Individual low-level alerts may not always represent a serious incident.
Correlation helps identify patterns across multiple events.
Recommended Process
Review individual alerts.
Identify repeated patterns.
Compare source IP addresses.
Compare usernames.
Review timestamps.
Correlate related events.
Escalate when thresholds are reached.
5. Incident Triage
Each security alert should be reviewed using this process:
Detection
Validation
Context Collection
Severity Assessment
Investigation
Containment
Remediation
Recovery
Documentation
6. False Positive Handling
Not every alert represents malicious activity.
Before taking action:
Confirm the event.
Identify the source.
Review surrounding activity.
Check whether the activity was authorized.
Document the reason if the alert is considered benign.
7. Continuous Improvement
Detection rules should be reviewed regularly.
Possible improvements include:
Adjusting thresholds.
Adding new correlation rules.
Mapping more alerts to MITRE ATT&CK.
Reducing false positives.
Monitoring additional log sources.
Adding endpoint agents.
Creating alert dashboards.
8. Security Monitoring Recommendations
A production environment should include:
Centralized log collection
Endpoint monitoring
File Integrity Monitoring
Authentication monitoring
Network monitoring
Custom detection rules
Alert correlation
Incident response procedures
Regular detection testing
9. Retesting
After remediation, recreate the original activity where appropriate.
Confirm that:
Security controls are working.
Alerts still detect suspicious behavior.
Legitimate activity does not generate excessive false positives.
Remediation does not break required services.