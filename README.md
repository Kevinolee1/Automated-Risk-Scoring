# Automated-Risk-Scoring
automatically assign LOW, MEDIUM, HIGH, or CRITICAL based on threat-intelligence results
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/f935674335877e8f18aa2a7873f1dd1f3e10687e/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20140751.png)
Add this function above the section where you ask for the IOC:
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/1b1b67ead50448568ee2e264f68d63dff8fb7233/Screenshot%202026-08-31%20173726.png)
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/e5d25490517652408c47eeabe91e8a939289c5f4/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20142117.png)
At the end of the successful section of investigate_ip(), add:

return stats.get("malicious", 0), stats.get("suspicious", 0)

it should look like

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/c0f3a32494c48574d6e7132e1efcb9b0f1b60de8/Screenshot%202026-08-31%20173751.png)

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/17e6a58d45accc34902058d96eb40a78a1c5e27a/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20142749.png)
For the error section, add: return 0, 0

It should look like this

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/3f09e884b007cfabeb4ac545dda626aaf8d68b56/Screenshot%202026-08-31%20173817.png)

![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/de49585462328977731c1448520e84cad05bbfef/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20143226.png)
At the end of the successful part of investigate_ip_abuseipdb(), add: return data.get("abuseConfidenceScore", 0)

print(f"ISP: {data.get('isp', 'Unknown')}")
print(f"Domain: {data.get('domain', 'Unknown')}")
print(f"Usage Type: {data.get('usageType', 'Unknown')}")

So it looks like:
return data.get("abuseConfidenceScore", 0)
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/fa948d70ae43bbfa94661ce55d8f067a2a547987/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20143852.png)
Find:
elif ioc_type == "IP Address":
    investigate_ip(ioc)
    investigate_ip_abuseipdb(ioc)

Replace it with:
elif ioc_type == "IP Address":
    vt_malicious, vt_suspicious = investigate_ip(ioc)

    abuse_score = investigate_ip_abuseipdb(ioc)

    risk_level = calculate_risk(
        vt_malicious,
        vt_suspicious,
        abuse_score
    )

    print()
    print("SOC Risk Assessment")
    print("-------------------")
    print(f"Risk Level: {risk_level}")

Press Ctrl+S to save 
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/670cac35a7de726564b9cdf39a6e6e4ee852cbcd/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20144926.png)
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
Risk Level: LOW

For this lab, we're defining:

Risk	Logic
LOW	0 malicious, 0 suspicious, AbuseIPDB below 25%
MEDIUM	1+ VT malicious/suspicious or AbuseIPDB ≥25%
HIGH	5+ VT malicious or AbuseIPDB ≥70%
CRITICAL	10+ VT malicious or AbuseIPDB ≥90%
