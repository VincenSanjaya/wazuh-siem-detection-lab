File Integrity Monitoring Detection
Objective:
Detect file creation, modification, and deletion using Wazuh File Integrity Monitoring.
Monitored Directory:
/tmp/wazuh-lab
Test File:
/tmp/wazuh-lab/test.txt
Test Activity:
File created:
echo "test security event" > /tmp/wazuh-lab/test.txt
File modified:
echo "modified content" >> /tmp/wazuh-lab/test.txt
File deleted:
rm /tmp/wazuh-lab/test.txt
Detection Results:
File Added
Rule ID: 554
Rule Level: 5
Description: File added to the system.
File Modified
Rule ID: 550
Rule Level: 7
Description: Integrity checksum changed.
File Deleted
Rule ID: 553
Rule Level: 7
Description: File deleted.
Analysis:
Wazuh successfully detected all three file system activities in the monitored directory.
The modification and deletion events received higher rule levels than the file creation event.
This demonstrates how File Integrity Monitoring can be used to detect unauthorized or suspicious changes to important files.
Potential Security Impact:
Unexpected changes to sensitive files may indicate:
Unauthorized configuration changes
Malware activity
Persistence mechanisms
Tampering with application files
Deletion of security-related files
Remediation:
Review unexpected file modifications.
Restrict write permissions on sensitive directories.
Use least privilege.
Investigate the user or process responsible for the change.
Maintain backups of critical files.
Evidence:
screenshots/fim-alerts.png