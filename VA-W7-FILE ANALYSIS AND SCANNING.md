# VA-W7-LAB-FILE ANALYSIS AND SCANNING WRITEUP

## TOOLS USED

## QUESTION 1: FIND THE FLAG (Wireshark)
Analysing the PCAP file for Question 1 provided 41 packets of data of the ICMP protocol. 

After browsing through, Packet 37 was the only Packet with a length of 70 and after viewing its data there seems to be a suspicious long 
string of text: `U1VDVEYyMDIze2FpX2lzX2Nvb2x9`

After enabling the 'Show Packet Bytes' feature in Wireshark I decoded the text as Base64 to ASCII which led to the first flag: `SUCTF2023{ai_is_cool}`

---

<img width="1567" height="1000" alt="Screenshot 2026-04-30 at 7 04 38 PM" src="https://github.com/user-attachments/assets/8fb4bbfd-1cbf-4baf-8297-cb43a39282f6" />


<img width="1569" height="1002" alt="Screenshot 2026-04-30 at 7 04 50 PM" src="https://github.com/user-attachments/assets/09681f77-8c9c-4e60-a6e1-9d35698f38e5" />


<img width="1045" height="758" alt="Screenshot 2026-04-30 at 7 06 26 PM" src="https://github.com/user-attachments/assets/e0a9784e-844f-462d-bfcf-14f4a2736f56" />


<img width="1045" height="755" alt="Screenshot 2026-04-30 at 7 06 37 PM" src="https://github.com/user-attachments/assets/6d617719-aa97-4120-a7a6-2082b85df1cb" />

---



## QUESTION 2: FIND THE FLAG (Wireshark)

---


## QUESTION 3: NMAP INTERPRETATION
### What can an attacker do with each port?
- **Port 21 (FTP):** An attacker can try logging in by providing default or anonymous username and password credentials for reading or modifying files on the system. In case an attacker manages to log in, he/she will be able to upload malicious scripts or download confidential information from the system.
- **Port 22 (SSH):** An attacker can try bruteforcing the username and password combination for obtaining access to a remote interactive shell. They could also try to enumerate valid usernames.
- **Port 80 (HTTP):** An attacker can visit the website hosted on the server to search for vulnerabilities in the web application installed on the webserver, look for the presence of administrative folders, or initiate a Denial of Service attack against it.
- **Port 139 & 445 (SMB/NetBIOS):** An attacker can enumerate network shares, list users, and attempt to exploit network file sharing protocols to move laterally or execute code remotely.

### What vulnerabilities are likely present based on the version?
- **vsftpd 2.3.4:** This specific version is infamous for a Malicious Backdoor (CVE-2011-2523). If an attacker attempts to log in with a username ending in a smiley face :), the server automatically opens a root shell on port 6200.
- **OpenSSH 5.3p1:** This is a very old version (released around 2009). It is likely vulnerable to user enumeration attacks and potentially older memory corruption bugs, though it is less critically exploitable than the others on this list.
- **Apache 2.2.8:** Released in 2008, this version is highly outdated and susceptible to numerous vulnerabilities, most notably the Slowloris DoS attack (CVE-2007-6750), which can easily crash the web server.
- **Windows 7 Professional SP1 (via Port 445):** Because SMB is open on an unpatched Windows 7 SP1 machine, it is almost certainly vulnerable to MS17-010 (EternalBlue).

### Which one is the highest risk and why?
- Port 445 (SMB) / Windows 7 SP1 represents the highest risk.
- This is because the EternalBlue (MS17-010) vulnerability allows an attacker to achieve Remote Code Execution (RCE) with highest system privileges (NT AUTHORITY\SYSTEM) without needing any usernames or passwords. It is a highly reliable exploit that can compromise the machine instantly over the network. Port 21 (vsftpd) is a close second due to its backdoor.

### What attack path can be built from this?
A typical attacker would follow this path:
- **Reconnaissance:** Run this Nmap scan and identify the Windows 7 SP1 OS with port 445 open.
- **Exploitation:** Launch the Metasploit framework and use the `exploit/windows/smb/ms17_010_eternalblue` module against the target IP.
- **Privilege Escalation:** None needed; EternalBlue grants immediate SYSTEM access.
- **Post-Exploitation:** Dump password hashes from memory (using Mimikatz or hashdump) to see if those credentials can be used to log into other machines on the network, or install a persistent backdoor.

### What should be the remediation?
- **Patching/Upgrading:** The most critical step is upgrading the operating system. Windows 7 reached its End of Life (EOL) years ago and no longer receives security updates. If it cannot be upgraded, the MS17-010 security patch must be applied immediately.
- **Service Updates:** Update vsftpd, OpenSSH, and Apache to their latest stable versions to remove the known exploits.
- **Firewall Rules:** Block port 445 and 139 at the perimeter firewall so they are not exposed to the public internet. SMB should only be accessible internally (if at all).
- **Disable Legacy Protocols:** Disable SMBv1 on the Windows machine entirely.

---

## QUESTION 4: IDENTIFY THE OS (OS Fingerprinting) - TTL

### IMAGE 1
<img width="504" height="176" alt="Screenshot 2026-04-30 at 8 11 54 PM" src="https://github.com/user-attachments/assets/914885a3-cdd2-4c48-b938-20fe660acb3f" />

- **ANSWER:** Linux / Unix / macOS
- **Reasoning:** The ping output shows a `ttl=64`. A default TTL (Time to live) of 64 is the standard starting value for most Linux distributions, macOS, and other Unix-like operating systems.

---

### IMAGE 2
<img width="281" height="194" alt="Screenshot 2026-04-30 at 8 12 18 PM" src="https://github.com/user-attachments/assets/72bca447-b27a-4a49-b134-55eca046fd28" />

- **ANSWER:** Network Device (e.g., Cisco Router/Switch)
- **Reasoning:** The Wireshark packet details show Time to live: 255. A starting TTL of 255 is predominantly used by network infrastructure hardware, particularly Cisco IOS devices.

---

### IMAGE 3
<img width="516" height="116" alt="Screenshot 2026-04-30 at 8 12 45 PM" src="https://github.com/user-attachments/assets/c0a81c1c-01db-44da-8a71-5491c9096b1f" />

- **ANSWER:** Windows
- **Reasoning:** The ping replies show `ttl=128`. A default TTL of 128 is the standard starting value for Microsoft Windows operating systems.

---



---

## QUESTION 5: ANALYSE THE NESSUS FILE
Upload to your nessus (Network_Scan.nessus) and analyse the files. Focus on critical or high findings that was identifies in analysis named “Ghostcat”.

---

<img width="1575" height="1011" alt="Screenshot 2026-04-30 at 8 44 26 PM" src="https://github.com/user-attachments/assets/566aefb0-760a-4186-99b9-7bce64eeb5d6" />

---

### What is the affected Port number?
### What is the Affected protocol?
### What is the CVSS Score of vulnerability found?
### Can you find any exploit related to this vulnerability?
### Find CVE for this vulnerability.

