# TryHackMe: Hydra Room Writeup 
A comprehensive guide to performing network login brute-forcing using Hydra, covering Web and SSH exploitation.

## Tools Used

| Tool | Category | Description |
| :--- | :--- | :--- |
| **Hydra** | Brute-Forcer | A parallelized login cracker which supports numerous protocols to attack. |
| **RockYou.txt** | Wordlist | A famous password list containing over 14 million common passwords. |
| **Browser DevTools** | Enumeration | Used to inspect HTTP requests and identify form parameters. |
| **SSH Client** | Remote Access | Used to gain shell access once credentials are recovered. |

---

## Step-by-Step Execution

### Phase 1: Web Form Brute Force
The first objective is to crack the login page on the web server.

#### 1. Analyze the Form
Before running the command, we inspect the page to find the **POST** parameters.
* **Target URL:** `http://<MACHINE_IP>/login`
* **Form Data:** `username=^USER^&password=^PASS^`
* **Failure Message:** `Your username or password is incorrect.`

#### 2. Execute Hydra
Run the following command to crack the web login:


- hydra -l molly -P /usr/share/wordlists/rockyou.txt <MACHINE_IP> http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." -V -t 4

**Technical Breakdown:**
- -l molly: Sets the static username to "molly".
- -P .../rockyou.txt: Points to the password wordlist.
- http-post-form: Specifies the module for web-based logins.
- "/login...F=...": This string tells Hydra where to post, what data to send, and that the login Failed if the specific error message is seen.

We are basically telling Hydra: "Go to this website, try every password in this list for the user 'molly.' If you see the words 'incorrect,' you failed—keep going. If you don't see those words, you found the password!"



### Phase 2: SSH Exploitation
Once we have a lead, we check if the user "molly" reused her credentials or has other passwords for SSH access.

#### 1. Execute Hydra for SSH

`hydra -l molly -P /usr/share/wordlists/rockyou.txt <MACHINE_IP> ssh -t 4 -V`

**Technical Breakdown:**
- ssh: Directs Hydra to use the Secure Shell protocol (Port 22).
- -t 4: Limits parallel connections to 4. SSH servers often have "MaxStartups" settings that will block you if you try too many connections at once.
- -V: Verbose mode—shows every login attempt.
We are knocking on the machine's "back door" (SSH) very quickly with a list of keys. We limit the speed so the machine doesn't get overwhelmed and stop talking to us.

### Phase 3: Post-Exploitation (Flag Recovery)
Now that we have the credentials, we log in to claim the flags.

#### 1. SSH Login
`ssh molly@<MACHINE_IP>`
- Enter password obtained: `butterfly`
#### 2. Locate Flags
`ls`
- list files in the home directory
#### 3. Read the final flag contents
`cat flag2.txt` 


