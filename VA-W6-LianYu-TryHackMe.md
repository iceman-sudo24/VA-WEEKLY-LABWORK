# VA-W6-LianYu-TryHackMeRoom
- Write up for the LianYu machine in TryHackMe
- 
## 🚩 Room Information
- **Target OS:** Linux
- **Difficulty:** Easy
- **Objective:** Capture the User and Root flags through enumeration, steganography, and privilege escalation.
- **Target IP:** 10.48.187.151
---

## 🛠 Tools Used

| Tool | Purpose | Description |
| :--- | :--- | :--- |
| **Nmap** | Scanning | Network mapper used to discover open ports and services. |
| **Gobuster** | Enumeration | A tool used to brute-force URIs (directories/files) on web servers. |
| **FTP Client** | Gaining Access | Used to interact with the File Transfer Protocol service on port 21. |
| **Hexeditor** | Steganography | Used to view and edit the raw bytes (headers) of files. |
| **Steghide** | Steganography | A tool used to hide or extract data from image and audio files. |
| **SSH** | Gaining Access | Secure Shell, used for remote login to the target machine. |

## 0. Prerequisite (LianYu Machine Connection)
- I connected to the TryHackMe network with the VPN configuration downloaded on their website straight into KaliLinux
- I started the machine in the LianYu Room and ran the command `sudo openvpn tryhackme.ovpn` in KaliLinux
- I verified I was connected with `ip addr` to see the ``tun0`` interface

**Connection**
<img width="321" height="325" alt="image" src="https://github.com/user-attachments/assets/c6087bee-f28d-4877-b88b-7b6d3ee727b0" />

**Verification**
<img width="610" height="293" alt="image" src="https://github.com/user-attachments/assets/3b8c0a47-f80c-417a-8874-55de824f4b9e" />

## 1. Reconnaissance & Scanning
First a comprehensive Nmap scan is ran on the target IP to identify the attack surface.

- `nmap -sC -sV -p- 10.48.187.151`
- `-sC` Runs a collection of scripts from Nmap's Scripting Engine (NSE) to find common vulnerabilities and information
  - Retrieving SSL/TLS certificates.
  - Checking for anonymous FTP login.
  - Identifying web server directories.
  - Checking for basic, well-known misconfigurations.
- `-sV` Probes open ports to determine what software is running and its specific version number
- `-p-` Scans all 65,535 possible TCP ports instead of just the top 1,000 "common" ones

- --

<img width="678" height="505" alt="image" src="https://github.com/user-attachments/assets/db1ee4d3-116e-4ea9-a245-6589cd9628bd" />

- --

- **RESULTS**
- A total of five ports were found
    - `Port 21: FTP (vsftpd 3.0.2)`
    - `Port 22: SSH (OpenSSH 6.7p1 Debian 5+deb8u8)`
        - OpenSSH 6.7 is outdated and may be susceptible to user enumeration or specific older CVEs, though it is 
          generally robust unless credentials are weak
    - `Port 80: HTTP (Apache 2.4.7)`
      - Site Title: "Purgatory"
      - The server is running Apache. The custom title suggests a web application is hosted here (Purgatory)
      - This is the primary attack surface where directory brute-forcing will be performed (using tools like
      gobuster to find hidden files or admin panels.
    - `Port 111/TCP & 36598/TCP — RPCBind / NFS (rpcbind 2-4 and status)`
      - RPCBind is often a precursor to NFS (Network File System). The rpcinfo script output shows several RPC
      services running.

## 2. Web Enumeration

Visiting the website on Port 80 shows a web page with a synopsis of the 'Arrowverse' TV Series which we can infer is the theme of the TryHackMe. This is where gobuster directory brute-forcing will be perforned to find hidden content.

- --

<img width="681" height="407" alt="image" src="https://github.com/user-attachments/assets/0c552be0-78d9-404d-8b1f-a0edb30996e4" />

- --

- Viewing the page source doesn't show any viable/explicit clues or information

<img width="686" height="332" alt="image" src="https://github.com/user-attachments/assets/d5d115cd-ce8b-4b42-8ec0-307965848496" />

- --

`gobuster dir -u http://10.49.187.151 -w /usr/share/wordlists/dirb/common.txt`
- `dir:` Tells Gobuster to use directory-enumeration mode.
- `-u:` Specifies the target URL (the "Purgatory" web server you found earlier).
- `-w:` Points to a wordlist. This is a text file containing thousands of common folder names (like /admin, /login, /backup). Gobuster tries each one to see if it exists. (in this case we're using common.txt)

- --

<img width="683" height="324" alt="image" src="https://github.com/user-attachments/assets/e50d7ad2-ace7-424e-b697-2c408ec8b2b5" />
- No useful results were found

- --

`gobuster dir -u http://10.49.187.151 -w /usr/share/wordlists/dirb/medium.txt -t 50`
- We change the wordlist to medium.txt as it has a bigger range of names to compare to
- `-t:` Was used to accelerate the enumeration process by specifying the number of CPU threads to use in the enumeration process

- --

<img width="689" height="229" alt="image" src="https://github.com/user-attachments/assets/e23d5c5f-c099-4d58-b04f-3d1055a03c28" />
- This time an `/island` directory was found which will be investigated

- --

<img width="380" height="245" alt="image" src="https://github.com/user-attachments/assets/30e91f30-1fb8-441d-8e5e-313c3c4152a5" />

- --

<img width="662" height="346" alt="image" src="https://github.com/user-attachments/assets/2ad0ee7d-7697-41e0-b637-8227d5dc7fca" />
- Viewing the page source revealed a code word: `Vigilante`

- --

Investigating Subdirectories
Checking the source code of /island revealed the codeword "vigilante". We then scan the next layer:

Bash
gobuster dir -u http://$IP/island -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 64
Found: /2100

Finding the Token
Inside /2100, we look for specific file extensions as hinted in the source code:

Bash
gobuster dir -u http://$IP/island/2100 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x .ticket -t 64
Found: green_arrow.ticket

Accessing the file reveals a Base58 encoded string: RTy8yhbQDscX.

3. Gaining Access
FTP Extraction
We decode the string (result: password) and use it to log into the FTP service with the username vigilante.

Bash
ftp $IP
# Username: vigilante
# Password: password
binary
mget *
Steganography Analysis
We retrieve three images. Leave_me_alone.png has a corrupt header. We fix it using hexeditor to match the standard PNG signature (89 50 4E 47 0D 0A 1A 0A).

The image reveals the passphrase: password.

Next, we extract hidden data from aa.jpg:

Bash
steghide extract -sf aa.jpg
# Passphrase: password
This gives us ss.zip. Unzipping this reveals a file containing the SSH password for the user slade.

SSH Login
Bash
ssh slade@$IP
4. Privilege Escalation
Once logged in as slade, we check for sudo permissions:

Bash
sudo -l
Observation: Slade can run /usr/bin/pkexec as root without a password.

Exploitation
Using the pkexec binary to spawn a root shell:

Bash
sudo /usr/bin/pkexec /bin/sh
Verification:

Bash
whoami  # Returns root
cat /root/root.txt
