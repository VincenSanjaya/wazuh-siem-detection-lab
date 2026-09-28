SSH Brute-Force Detection with Custom Correlation Rule
Objective:
Detect repeated failed SSH login attempts from the same source IP and generate a higher-severity alert.
Log Source:
/tmp/wazuh-auth.log
Base Detection:
Rule ID: 5710
Rule Level: 5
Description:
sshd: Attempt to login using a non-existent user
Custom Correlation Rule:
Rule ID: 100100
Rule Level: 10
Condition:
5 failed SSH login attempts
from the same source IP
within 60 seconds
MITRE ATT&CK:
T1110
Brute Force
Custom Rule:
<group name="custom_ssh,authentication_failed,">
  <rule id="100100" level="10" frequency="5" timeframe="60">
    <if_matched_sid>5710</if_matched_sid>
    <same_srcip />
    <description>Possible SSH brute-force attack: multiple failed logins from same source IP</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>

Test Activity:
Multiple failed SSH authentication events were generated from the same simulated source IP.
Example:
Sep 28 09:25:01 lab sshd[3001]: Failed password for invalid user admin from 192.168.56.30 port 52001 ssh2
Detection Result:
Wazuh first generated multiple Rule 5710 alerts.
After the configured threshold was reached, the custom correlation rule generated:
Rule ID: 100100
Rule Level: 10
Description:
Possible SSH brute-force attack: multiple failed logins from same source IP
Analysis:
The detection successfully correlated several low-severity authentication failures into one higher-severity brute-force alert.
This reduces alert noise and helps SOC analysts identify repeated authentication failures as a single security incident.
Investigation Steps:
Review the source IP.
Identify targeted usernames.
Review the number of authentication attempts.
Check for successful authentication after the failures.
Search for other events associated with the same source IP.
Review activity around the same timestamp.
Remediation:
Block or investigate suspicious source IP addresses.
Apply authentication rate limiting.
Use strong passwords.
Prefer SSH key authentication.
Disable direct remote access for unnecessary accounts.
Use multi-factor authentication where supported.
Evidence:
screenshots/failed-login-alerts.png
screenshots/custom-bruteforce-rule.png