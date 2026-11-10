# Multi-Cloud T-Pot Honeynet Study

Network Security (MACSH6011), Assignment 1, TU Dublin.
Author: Kanishk M Khobragade (A00083500)

Two T-Pot 24.04.1 (Hive) honeypots were deployed on Microsoft Azure and Amazon Web Services and compared over 17 to 31 October 2026 (UTC). Events were analysed in Kibana.

## Setup
| | Azure | AWS |
|---|---|---|
| Region | Spain Central (Zone 1) | Europe (Stockholm), eu-north-1 |
| Instance | Standard D2ds v4 (2 vCPU, 8 GiB) | EC2 m7i-flex.large (2 vCPU, 8 GiB) |
| OS | Ubuntu Server 26.04 LTS | Ubuntu Server 26.04 |
| First data | 17 Oct 2026 | 16 Oct 2026 |

Honeypots in T-Pot include Honeytrap, Heralding, SentryPeer, Cowrie, RDPHoneypot, Dionaea and Suricata.

## Key findings
1. **Azure recorded about 4x more events**: about 3 million on Azure against about 681,000 on AWS.
2. **Top destination ports differed.**
   - Azure: 5060 (SIP) 594,550; 5900 (VNC) 585,826; 3389 (RDP) 370,806; 22 (SSH) 71,162; 161 (SNMP) 55,595.
   - AWS: 5060 252,806; 3389 98,159; 110 (POP3) 32,958; 143 (IMAP) 20,299; 1433 (MS SQL) 17,260.
3. **A VNC campaign hit Azure.** Port 5900 events grew for five days and peaked at 124.6K on 23 Oct, then fell below 5K a day from 27 Oct. This looks like a single scanning campaign (a hypothesis).
4. **Attackers looked like rented cloud servers.** One address, 185[.]136[.]15[.]17, appeared on both servers. DigitalOcean (AS14061) was the top network on Azure. The top-10 addresses made up 34% of Azure events and 50% of AWS events.
5. **Weak and default credentials were tried.** Common usernames were root, Administrator, admin and sa. Common passwords were blank, 123456, password and admin123. These are attempts, not successful logins.
6. **MITRE ATT&CK mapping:** T1595 Active Scanning, T1110 Brute Force, T1021.005 VNC, T1021.001 RDP.

## Takeaways for defenders
- Scanning starts within hours of a server going online.
- Keep VNC, RDP, SNMP and SIP off the open internet.
- Use unique passwords and MFA.

## Limitations
Two servers, two weeks, low-interaction honeypots, approximate IP geolocation, and AWS started one day earlier. Suricata "CVE" counts are rule metadata, not proof of exploitation.

## Safety note
Server IP addresses, keys and credentials are deliberately not included in this repository. Attacker IPs are written in defanged form, for example `185[.]136[.]15[.]17`.
