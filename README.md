# Automated-Risk-Scoring
automatically assign LOW, MEDIUM, HIGH, or CRITICAL based on threat-intelligence results
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/f935674335877e8f18aa2a7873f1dd1f3e10687e/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20140751.png)
Add this function above the section where you ask for the IOC:
def calculate_risk(vt_malicious, vt_suspicious, abuse_score):
    if vt_malicious >= 10 or abuse_score >= 90:
        return "CRITICAL"

    elif vt_malicious >= 5 or abuse_score >= 70:
        return "HIGH"

    elif vt_malicious >= 1 or vt_suspicious >= 1 or abuse_score >= 25:
        return "MEDIUM"

    else:
        return "LOW"
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/e5d25490517652408c47eeabe91e8a939289c5f4/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20142117.png)
At the end of the successful section of investigate_ip(), add:
return stats.get("malicious", 0), stats.get("suspicious", 0)

it should look like
print(f"Country: {attributes.get('country', 'Unknown')}")
print(f"Network: {attributes.get('network', 'Unknown')}")
print(f"ASN: {attributes.get('asn', 'Unknown')}")
print(f"Owner: {attributes.get('as_owner', 'Unknown')}")

return stats.get("malicious", 0), stats.get("suspicious", 0)
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/17e6a58d45accc34902058d96eb40a78a1c5e27a/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20142749.png)
For the error section, add: return 0, 0

Like this
 else:
    print()
    print(f"IP lookup failed. Status Code: {response.status_code}")
    print(response.text)

    return 0, 0
![Image alt](https://github.com/Kevinolee1/Automated-Risk-Scoring/blob/de49585462328977731c1448520e84cad05bbfef/Automated%20Risk%20Scoring/Screenshot%202026-08-31%20143226.png)
At the end of the successful part of investigate_ip_abuseipdb(), add: return data.get("abuseConfidenceScore", 0)

print(f"ISP: {data.get('isp', 'Unknown')}")
print(f"Domain: {data.get('domain', 'Unknown')}")
print(f"Usage Type: {data.get('usageType', 'Unknown')}")

So it looks like:
return data.get("abuseConfidenceScore", 0)

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

Go back to PowerShell to type python main.py and press enter.  For enter IOC to investigate use 8.8.8.8
You should get 

(.venv) PS C:\Users\eelve\SOC-IOC-Investigation-Automation> python main.py
Enter IOC to investigate: 8.8.8.8

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
