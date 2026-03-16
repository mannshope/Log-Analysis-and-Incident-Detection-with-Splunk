# Log-Analysis-and-Incident-Detection-with-Splunk
📌 Project Overview
This project demonstrates basic Security Operations Center (SOC) analyst workflows using Splunk Enterprise. The goal was to ingest a login dataset, analyze successful vs. failed login attempts, and identify potential security threats through pattern recognition.

🛠️ Tools Used
Splunk Enterprise: Data ingestion, searching, and visualization.

Dataset: splunk_login_dataset.csv (Contains timestamps, usernames, source IPs, login status, and locations).

🔍 Investigation & Findings
1. Total Failed Login Attempts
To identify the scale of potential brute-force activity, I filtered the logs for "failed" status.

SPL Query: index="main" status="failed" | stats count

Observation: Highlighting failed attempts allows us to see the volume of rejected access.

2. Top Offending IP Address
I looked for the source IP responsible for the most failed logins to identify potential external attackers.

SPL Query: index="main" status="failed" | top src_ip

Finding: The IP 192.168.1.22 (and others) showed repeated failures, suggesting a targeted scan or brute-force attempt.

3. Most Frequent Username
Identifying the most targeted account helps determine if an attacker is focused on a specific identity.

SPL Query: index="main" | top username

Finding: Alice is the most frequent username appearing in the logs.

🚩 Suspicious Patterns Identified
During the analysis, one specific account stood out due to anomalous behavior:

Targeted Account: Alice.

The Anomaly: Not only is Alice the most frequent username, but her login events are frequently associated with "Unknown" locations.

Security Risk: This could indicate a Geographic Impossibility or a VPN/Proxy being used to mask an attacker's true origin. While Alice has the most activity, the lack of a registered location for her successful/failed attempts is a major red flag for a "Credential Stuffing" attack.

🚀 How to Reproduce
Upload the splunk_login_dataset.csv to your Splunk instance.

Set the source type to _json or csv (ensure headers are recognized).

Run the SPL queries provided in the "Investigation" section above.

Visualize the results using Statistics and Visualization tabs for better reporting.

💡 Conclusion
By analyzing these logs, I successfully identified a high-risk user (Alice) and a suspicious pattern involving unknown locations. This highlights the importance of monitoring not just who is logging in, but where they are coming from.
