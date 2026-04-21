# VA-W6-LianYu-TryHackMeRoom
- Write up for the LianYu machine in TryHackMe

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
- This time an **/island** directory was found which will be investigated for any clues

- --

<img width="380" height="245" alt="image" src="https://github.com/user-attachments/assets/30e91f30-1fb8-441d-8e5e-313c3c4152a5" />

- --

<img width="662" height="346" alt="image" src="https://github.com/user-attachments/assets/2ad0ee7d-7697-41e0-b637-8227d5dc7fca" />
- Fortunately, viewing the page source revealed a code word in the comments: `Vigilante`

- --

- Investigating further, gobuster was ran again to enumerate the /island directory to find more hidden layers/subdirectories

`gobuster dir -u http://10.49.187.151/island -w /usr/share/wordlists/dirb/medium.txt -t 50`

- The results of the scan revealed another directory **/2100**

<img width="598" height="363" alt="image" src="https://github.com/user-attachments/assets/628f9c51-094d-4683-b2cf-1eec222cb164" />

- --

Inside the page source of **/2100**, a comment was found that details a possible file extension to look out for

<img width="657" height="268" alt="image" src="https://github.com/user-attachments/assets/876a7674-6651-45d7-938f-8f0c7f544bf6" />

- --

- Gobuster is used again to further enumerate to find more subdirectories
- `gobuster dir -u http://10.49.187.151/island -w /usr/share/wordlists/dirb/medium.txt -x .ticket -t 50`
  - The `-x` flag is used to look out for specific file extensions, in this case `.ticket`
- The results of the scan found the file **green_arrow.ticket**

<img width="683" height="209" alt="image" src="https://github.com/user-attachments/assets/43ee09e7-3b1f-4a65-88d9-2ca448dd722e" />

- --

Accessing the file reveals a Base58 encoded string: RTy8yhbQDscX which will have to be decoded to view its true nature

<img width="640" height="303" alt="image" src="https://github.com/user-attachments/assets/6d0c1d58-8784-451b-b22f-4e0667bd3ab3" />

- --

## 3. Gaining Access
### Decoding Credentials
- Taking the Base58 encoded string we found from the `green_arrow.ticket` file, we decode it with an echo command
  - `echo "RTy8yhbQDscX" | base58 -d`
- This revealed the string of text `!#th3h00d` which is possibly the password to use along with the username we discovered previously `vigilante`

<img width="335" height="64" alt="image" src="https://github.com/user-attachments/assets/8b56ce19-46cb-4aa0-894d-ec244db0755f" />

- --
### FTP Exploitation

- We connect to the FTP server with `ftp 10.49.187.151` and attempt the login with the username and password 
- `Username: vigilante`
- `Password: !#th3h00d`

After successfully logging in the command `ls -la` was used to list out everything in the current directory
- The `-l` option stands for "long format" and provides detailed information about each file and directory. 
- The `-a` option includes hidden files in the listing.





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

## 4. Privilege Escalation
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
