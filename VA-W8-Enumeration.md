# VA-W8-ENUMERATION
**OBJECTIVE**
- Do a minimum of 10 enumeration challenges out of 30 with Windows as the Attacker and Metasploitable2 as the Victim

---

# BACKGROUND

| Tool | Purpose | Description |
| :--- | :--- | :--- |
| **Zenmap** | Scanning | Open source and free official security scanner GUI for NMAP // This is downloaded in my Windows 11 to be used in this week's lab |
| **Metasploitable2** | Vulnerable Machine | Metasploitable 2 is an intentionally vulnerable Linux virtual machine designed for security training and testing |

**ATTACKER (Windows)**
- Attacker Host IP: `192.168.64.4`
- Attacker OS: Windows 11

**VICTIM (Metasploitable2)**
- Victim IP: `192.168.64.3` 
- Victim OS: Linux

**What is enumeration?**
- Enumeration is the systematic process of extracting detailed information from a target system, such as usernames, hostnames, network shares, and service versions, by establishing active connections to its open ports. In the context of this lab, it serves as the bridge between initial scanning and actual exploitation, allowing a security professional to map out the attack surface and identify specific misconfigurations or vulnerabilities in services like SMB, FTP, and RPC.

# PREREQUISITES
1. Download and prepare a Windows (10 or 11) machine to serve as the attacker
2. Download and prepare Metasploitable2 to serve as the victim
3. Download Zenmap on the Windows attacker machine
4. Start both machines up and ping the victim IP on the attacker machine to ensure there is a 
connection

--- 

## Attacker and Victim IP's
<img width="512" height="333" alt="unnamed (1)" src="https://github.com/user-attachments/assets/d9af7998-b44b-44f4-87de-05fa449aa5c8" />
<img width="512" height="301" alt="unnamed" src="https://github.com/user-attachments/assets/715c4c9f-deb4-4608-a4ae-7eea9f8430e5" />

---

## 1. Challenge 1 - NetBIOS Enumeration
<img width="520" height="360" alt="Screenshot 2026-05-09 at 4 13 58 AM" src="https://github.com/user-attachments/assets/45975799-fa81-4885-b5ca-fd65e5f14ea5" />

- `nbtstat` is a Windows command-line utility used to display NetBIOS over TCP/IP (NetBT) statistics, name tables, and cached names to troubleshoot network and name resolution issues.
- `-a` Stands for "adapter status." It tells Windows to retrieve the NetBIOS name table from the remote device at that IP.

---

## 2. Challenge 2 - Fast Nmap Scan
<img width="677" height="678" alt="Screenshot 2026-05-09 at 4 18 38 AM" src="https://github.com/user-attachments/assets/46d4383a-dc26-4bb9-8b1a-3a39727ad87d" />

- `-F` Stands for "Fast." It limits the scan to the 100 most common ports instead of the default 1,000.

---

## 3. Challenge 3 - DNS Records
<img width="591" height="368" alt="Screenshot 2026-05-09 at 4 44 46 AM" src="https://github.com/user-attachments/assets/88c37f36-4462-4065-957a-5bd15fcc40b7" />
-` nslookup:` is a network administration tool used to query the Domain Name System (DNS) to obtain domain name or IP address mapping information. It is commonly used to diagnose DNS infrastructure issues and verify DNS records.

---

## 4. Challenge 5 - TTL OS Fingerprinting
<img width="578" height="299" alt="Screenshot 2026-05-09 at 4 20 56 AM" src="https://github.com/user-attachments/assets/03a44421-5525-420d-b717-c79aff01df4a" />

- `ping` Sends an ICMP Echo Request. In the response, the TTL (Time to Live) value is inspected to guess the OS (64 for Linux, 128 for Windows).
- By comparing this value to known defaults, you can infer the probable OS. 
  - For example, Windows typically starts with TTL 128, while Linux/Unix/macOS often use 64
 
- Values are as expected as we can see the TTL (Time to live) value to be 64 which suggests that the OS of the victim is Linux / Unix (which matches with the victim's OS)

---

## 5. Challenge 10 - Anonymous FTP Login
<img width="499" height="354" alt="Screenshot 2026-05-09 at 4 33 12 AM" src="https://github.com/user-attachments/assets/32255a46-9032-4d69-b5a1-599b4c6682cd" />

- I reached the expecteed outcome as the login succeeded and I was able to use the `ls` command to list readable directories on the victim
- `ftp` Launches the interactive File Transfer Protocol client to establish a connection. (Followed by typing anonymous as the user).

---

## 6. Challenge 13 - NFS Exports
<img width="644" height="423" alt="Screenshot 2026-05-09 at 4 58 09 AM" src="https://github.com/user-attachments/assets/a06b6f35-696d-4582-ba0a-d00a2cb8138f" />

- `showmount -e <IP>` did not work on my windows cmd so I opted to an nmap command
- `nmap -p 111 --script nfs-showmount 192.168.64.3`
  - `-p 111:` Targets the RPC Portmapper service, which handles the mounting requests for NFS.
  - `--script nfs-showmount` Executes a script that mimics the showmount -e command to list all exported (shared) directories on the victim.

---

## 7. Challenge 16 - Version Detection
<img width="878" height="676" alt="Screenshot 2026-05-09 at 4 28 58 AM" src="https://github.com/user-attachments/assets/33caacb0-4c3d-4ccd-8f77-345307488ba8" />

- `-sV` Probes all discovered open ports to find version strings (e.g., Apache 2.2.8) for the report.

---

## 8. Challenge 17 - OS Detection
<img width="841" height="795" alt="Screenshot 2026-05-09 at 4 40 08 AM" src="https://github.com/user-attachments/assets/eaa51454-4fec-4ecb-8f0c-527c33257008" />

- `Linux 2.6.9 - 2,6,33` was the version of the victim's OS
- `-O` (Capital letter O) Enables OS Detection. Nmap sends packets and compares the response "fingerprint" to a database of known operating systems.
  
---

## 9. Challenge 19 - RPC Info (Remote Procedure Call)
<img width="880" height="676" alt="Screenshot 2026-05-09 at 4 36 15 AM" src="https://github.com/user-attachments/assets/275bf636-82f1-4f0a-b904-734dfce23ca1" />

- `rpcinfo -p <IP>` did not work in my windows cmd prompt so I opted for an nmap command equivalent instead
- `nmap -p 111 --script rpcinfo 192.168.64.3`

---

## 10. Challenge 11 - SMB NSE Enumeration
<img width="878" height="676" alt="Screenshot 2026-05-09 at 4 35 05 AM" src="https://github.com/user-attachments/assets/4d9ddbb7-9f95-4a5d-9a88-618ebb88dbf6" />
<img width="632" height="816" alt="Screenshot 2026-05-09 at 4 52 24 AM" src="https://github.com/user-attachments/assets/6290fdae-67f1-48e4-9837-25548c522373" />
<img width="625" height="857" alt="Screenshot 2026-05-09 at 4 54 04 AM" src="https://github.com/user-attachments/assets/544f813f-9b74-4273-a105-42f22c584ef7" />
<img width="602" height="860" alt="Screenshot 2026-05-09 at 4 54 39 AM" src="https://github.com/user-attachments/assets/8109e3e4-4bbd-4dd8-805a-b3f89698c2f5" />

- `--script` tells Nmap to use a specific NSE (Nmap Scripting Engine) script. 
- `smb-enum-users` is the script used to extract the user list.
