WebStrike — Web Server Compromise Investigation

Overview
This investigation analyzes network traffic captured from a suspected compromise of a company web server. The objective was to determine the source of the attack, identify any malicious activity, analyze attacker persistence mechanisms, and detect potential data exfiltration. Analysis of the PCAP file was conducted using Wireshark to reconstruct attacker behavior and identify indicators of compromise.


Scenario
A suspicious file was discovered on an internal web server, prompting concerns of unauthorized activity. The network team captured network traffic and provided a PCAP file for forensic analysis. The goal of the investigation was to determine how the file was uploaded, identify any exploitation attempts, and assess whether sensitive data was accessed or exfiltrated.


Tools Used
Wireshark
HTTP Stream Analysis
TCP Stream Analysis
IP Geolocation Lookup


Platform Used
CyberDefenders — WebStrike Lab


Analysis
Review of the PCAP file identified suspicious HTTP traffic originating from external IP address 117.11.88.124 communicating with the web server at 24.49.63.79. GeoIP lookup determined the source of the activity originated from Tianjin, China, indicating an external attacker.

Inspection of HTTP request headers revealed the attacker utilized the following User-Agent string:

Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0

Further examination of HTTP POST requests identified two file upload attempts. The first attempt attempted to upload a file named image.php which failed due to server-side file validation. A second upload attempt used the filename image.jpg.php, successfully bypassing validation controls and resulting in successful upload of a malicious web shell.

Subsequent HTTP traffic revealed that uploaded files were stored within the /reviews/uploads/ directory, confirming the location of the deployed web shell.

Analysis of the uploaded file contents revealed a reverse shell command utilizing Netcat to establish outbound communication to the attacker’s system at IP address 117.11.88.124 over port 8080. This indicates the attacker successfully established command and control access to the compromised server.

Further investigation of TCP traffic over port 8080 revealed the attacker executing commands remotely. During this session, a curl command was observed attempting to transmit the /etc/passwd file to an external server, indicating an attempt to exfiltrate sensitive system information.


Findings
Attacker IP Address
117.11.88.124
Attacker Location
Tianjin, China
Attacker User-Agent
Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Malicious Web Shell
image.jpg.php
Upload Directory
/reviews/uploads/
Reverse Shell Port
8080
File Targeted for Exfiltration
/etc/passwd


MITRE ATT&CK Mapping
Initial Access — Exploitation of Public-Facing Application
Execution — Command Execution via Web Shell
Persistence — Web Shell Deployment
Command and Control — Reverse Shell over Netcat
Exfiltration — Data Exfiltration via HTTP


Conclusion
Network traffic analysis confirmed that an external attacker exploited a vulnerable file upload mechanism to deploy a malicious PHP web shell on the web server. The attacker bypassed validation controls using a double extension technique and established a reverse shell connection over port 8080. Following successful access, the attacker attempted to exfiltrate sensitive system information, specifically the /etc/passwd file. This activity confirms full compromise of the web server and highlights insufficient file upload validation and lack of outbound traffic monitoring.


Skills Demonstrated
Network Forensics
PCAP Analysis
Web Attack Investigation
Reverse Shell Detection
Data Exfiltration Detection
Threat Analysis
Incident Reconstruction