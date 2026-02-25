# Active Directory Penetration Testing Lab

## Overview
Simulated Active Directory penetration test demonstrating common attack techniques in a controlled lab environment.

## Lab Environment
- **Domain Controller:** Windows Server 2022
- **Workstations:** 2x Windows 10  
- **Attacker:** Kali Linux
- **Network:** 192.168.100.0/24

### 1. Initial Access
- LLMNR/MDNS Poisoning (Responder) → captured NTLMv2 hash
- Hash cracking (Hashcat + rockyou.txt) → plaintext credential
- SMB Relay Attack (ntlmrelayx) → SAM hashes dump

### 2. Post-Exploitation / Lateral Movement  
- IPv6 Attack (mitm6 + ntlmrelayx) → LDAPS relay → DCSync rights

### 3. Enumeration
- BloodHound domain mapping + attack path analysis

### 4. Privilege Escalation
- Kerberoasting → TGS hash → cracked (Hashcat)
- Pass-the-Hash → SYSTEM shell (smbexec)

### 5. Domain Compromise
- NTDS.dit dump → full domain credential dump
  
## Tools Used
- Kali Linux
- Responder
- Impacket (ntlmrelayx, smbexec, secretsdump)
- netexec
- BloodHound
- mitm6
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
