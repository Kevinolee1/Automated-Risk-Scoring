# Automated-Risk-Scoring
Built a Python-based automated risk-scoring tool that correlates VirusTotal and AbuseIPDB threat-intelligence results to classify IP addresses as LOW, MEDIUM, HIGH, or CRITICAL based on project-defined risk thresholds.
**Note:** The risk thresholds used in this project are custom scoring criteria created for this lab and are not official VirusTotal, AbuseIPDB, or industry-standard severity ratings.
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/f935674335877e8f18aa2a7873f1dd1f3e10687e/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20140751.png)
Add this function above the section where you ask for the IOC:

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/de299448c5230e0a87535afe584119dc787a6311/Screenshot%202026-08-31%20181028.png)

At the end of the successful section of investigate_ip(), add:

return stats.get("malicious", 0), stats.get("suspicious", 0)

it should look like

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/c0f3a32494c48574d6e7132e1efcb9b0f1b60de8/Screenshot%202026-08-31%20173751.png)

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/17e6a58d45accc34902058d96eb40a78a1c5e27a/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20142749.png)
For the error section, add: return 0

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/de49585462328977731c1448520e84cad05bbfef/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20143226.png)
At the end of the successful part of investigate_ip_abuseipdb(), 

add: return data.get("abuseConfidenceScore", 0)

So it looks like:

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/e56b05f70e859e15bf2f880ec9c82de8984db623/Screenshot%202026-08-31%20180423.png)

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/fa948d70ae43bbfa94661ce55d8f067a2a547987/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20143852.png)
Find:
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/a55ce50c264ce672067b12ecdcaeb6a727adceb4/Screenshot%202026-08-31%20173846.png)

Replace it with:
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/7b40db1b85ff61f453e00dbe9a2f40785608256e/Screenshot%202026-08-31%20174146.png)

Press Ctrl+S to save 
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/670cac35a7de726564b9cdf39a6e6e4ee852cbcd/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20144926.png)
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/df2b81aa293ec1529b5d88e60229c21758c8289c/Screenshot%202026-08-31%20182302.png)
Go back to PowerShell to type python main.py and press enter.  For enter IOC to investigate use 8.8.8.8
You should get 

SOC IOC Investigation Tool
--------------------------
IOC:  8.8.8.8
Type: IP Address

VirusTotal IP Results
---------------------
Malicious:  0
Suspicious: 0
Harmless:   53
Undetected: 38

IP Information
--------------
Country: US
Network: 8.8.8.0/24
ASN: 15169
Owner: Google LLC

AbuseIPDB Results
------------------
Abuse Confidence Score: 0%
Total Reports: 195
Country: US
ISP: Google LLC
Domain: google.com
Usage Type: Content Delivery Network

SOC Risk Assessment
-------------------
Risk Level: UNKNOWN

Reason: Insufficient threat-intelligence data to calculate risk.

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/535d48ce71a9555758b081978c1a42a0a83598a8/Screenshot%202026-08-31%20210650.png)

Open main.py in VS Code
Look for
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/be74049d0ad9e4165131079105bffdc8607f88fc/Screenshot%202026-08-31%20210156.png)

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/f8fe7a017cdc8c8d00840e45b4b61972a3aec1fd/Screenshot%202026-08-31%20211019.png)

Directly under the function, add:

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/56004bc8307cfc5589d92b7e96321b1c56ba6ea7/Screenshot%202026-08-31%20210323.png)

In PowerShell run: python main.py

You should see:

Risk Scoring Tests
------------------
LOW

MEDIUM

HIGH

CRITICAL

That confirms your calculate_risk() function correctly handles all four project-defined severity levels.
For this lab, we're defining:

Risk	Logic
LOW	0 malicious, 0 suspicious, AbuseIPDB below 25%

MEDIUM	1+ VT malicious/suspicious or AbuseIPDB ≥25%

HIGH	5+ VT malicious or AbuseIPDB ≥70%

CRITICAL	10+ VT malicious or AbuseIPDB ≥90%

Skills Demonstrated: Python | SOC Automation | Risk Assessment | Threat Intelligence | VirusTotal API | AbuseIPDB API | IOC Analysis | Data Correlation | Decision Logic | REST APIs |
