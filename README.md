# Active Directory Penetration Testing Lab

## Overview
Simulated Active Directory penetration test demonstrating common attack techniques in a controlled lab environment.

## Lab Environment
- **Domain Controller:** Windows Server 2022
- **Workstations:** 2x Windows 10  
- **Attacker:** Kali Linux
- **Network:** 192.168.100.0/24

## Attack Chain

### 1. Initial Access
- LLMNR Poisoning (Responder)
- SMB Relay Attack
- Hash cracking

### 2. Enumeration  
- BloodHound domain mapping
- Attack path analysis

### 3. Privilege Escalation
- Kerberoasting
- Pass-the-Hash lateral movement

### 4. Domain Compromise
- NTDS.dit dump
- Full domain takeover

## Tools Used
- Kali Linux
- Responder
- Impacket
- BloodHound
- Mimikatz
- Hashcat

## Key Findings
| Vulnerability | Severity |
|--------------|----------|
| LLMNR Enabled | High |
| Weak Service Passwords | Critical |
| SMB Signing Disabled | High |

## Mitigations
- Disable LLMNR/NBT-NS
- Enable SMB signing
- Strong password policy
- Network segmentation

## Disclaimer
Educational purposes only. Performed in isolated lab environment.

## References
- [TCM Security - PEH](https://academy.tcm-sec.com/p/practical-ethical-hacking-the-complete-course)
- [MITRE ATT&CK](https://attack.mitre.org/)
