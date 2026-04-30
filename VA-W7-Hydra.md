# VA-W7 TryHackMe: Hydra Room Writeup 
A comprehensive guide to performing network login brute-forcing using Hydra, covering Web and SSH exploitation.

## Tools Used

| Tool | Category | Description |
| :--- | :--- | :--- |
| **Hydra** | Brute-Forcer | A parallelized login cracker which supports numerous protocols to attack. |
| **RockYou.txt** | Wordlist | A famous password list containing over 14 million common passwords. |
| **Browser DevTools** | Enumeration | Used to inspect HTTP requests and identify form parameters. |
| **SSH Client** | Remote Access | Used to gain shell access once credentials are recovered. |

## Target Profile
- **NAME:** Hydra
- **IP:** 10.49.154.94

---

## Step-by-Step Execution

### Phase 1: Web Form Brute Force
The first objective is to crack the login page on the web server.

### 1. Analyze the Form
Before running the command, we inspect the page to find the **POST** parameters.
* **Target URL:** `http://<10.49.154.94>/login`
* **Form Data:** `username=^USER^&password=^PASS^`
* **Failure Message:** `Your username or password is incorrect.`

---

- We load up the site on the browser and use test/test for the login to find the post parameters
<img width="618" height="366" alt="Screenshot 2026-04-29 at 8 12 40 PM" src="https://github.com/user-attachments/assets/41f68196-39d0-4086-81aa-83c7f42343c5" />

---

- The result of the test credentials
<img width="617" height="384" alt="Screenshot 2026-04-29 at 8 14 51 PM" src="https://github.com/user-attachments/assets/e733b127-52a1-495f-beb0-f805ad79db30" />

---

- Viewing the page elements with Inspect and viewing the login requests
<img width="648" height="244" alt="Screenshot 2026-04-29 at 8 15 23 PM" src="https://github.com/user-attachments/assets/000a14b9-67f1-40a8-8d19-c03ef4175dae" />

- We obtain the raw data for the POST parameters in login
<img width="616" height="227" alt="Screenshot 2026-04-29 at 8 16 21 PM" src="https://github.com/user-attachments/assets/580d6fdd-4520-465b-bde8-8c01f49a3463" />

---

### 2. Execute Hydra
Run the following command to crack the web login:

`hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.49.154.94 http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." -v -t 4`

**Technical Breakdown:**
- `-l molly`: Sets the static username to "molly" (we get "molly" from the tryhackme page)
- `-P .../rockyou.txt`: Points to the password wordlist.
- `http-post-form`: Specifies the module for web-based logins.
- `"/login...F=..."`: This string tells Hydra where to post, what data to send, and that the login Failed if the specific error message is seen.
- `-v`: Verbose mode—shows every login attempt.

This is basically telling Hydra: "Go to this website, try every password in this list for the user 'molly.' If you see the words 'incorrect,' you failed—keep going. If you don't see those words, you found the password!"

---

- The result of the command 
<img width="623" height="303" alt="Screenshot 2026-04-29 at 8 17 55 PM" src="https://github.com/user-attachments/assets/743f7419-2451-4442-8aa1-18d569fad40c" />
<img width="582" height="116" alt="Screenshot 2026-04-29 at 8 18 10 PM" src="https://github.com/user-attachments/assets/0eaf2f2e-dc38-4f3b-bf9f-af78632bc852" />
- We get the password: `sunshine`

---

- Using `molly` as the username and `sunshine` as the password, we log into the website which presents us with the first flag.
<img width="1405" height="631" alt="Screenshot 2026-04-29 at 8 24 06 PM" src="https://github.com/user-attachments/assets/10fc0d53-abd6-4762-ba3d-51e113a3431b" />
**First Flag:** `THM{2673a7dd116de68e85c48ec0b1f2612e}`

----

### Phase 2: SSH Exploitation
Once we have the lead with the password, we check if the user "molly" reused her credentials or has other passwords for SSH access.

#### 1. Execute Hydra for SSH

`hydra -l molly -P /usr/share/wordlists/rockyou.txt <10.49.154.94> ssh -t 4 -V`

**Technical Breakdown:**
- `ssh`: Directs Hydra to use the Secure Shell protocol (Port 22).
- `-t 4`: Limits parallel connections to 4. SSH servers often have "MaxStartups" settings that will block you if you try too many connections at once.
- `-v`: Verbose mode—shows every login attempt.

We are knocking on the machine's "back door" (SSH) very quickly with a list of keys. We limit the speed so the machine doesn't get overwhelmed and stop talking to us.

---

<img width="1355" height="225" alt="Screenshot 2026-04-29 at 8 23 06 PM" src="https://github.com/user-attachments/assets/9057dde5-2c60-475a-ad21-f79101bc4144" />
- The result is the password `butterfly` which we can use for the SSH login

---

### Phase 3: Post-Exploitation (Flag Recovery)
Now that we have the credentials, we log in to claim the flags.

#### 1. SSH Login
- `ssh molly@<10.49.154.94>`: We establish and SSH connectiom with the IP and username and enter the password obtained: `butterfly` (I mistyped the first time which is why there is a re-prompt in the screenshot below)
#### 2. Locate Flags
- `ls`: list files in the home directory to see if there are any text files which can contain the final flag
#### 3. Read the final flag contents
- `cat flag2.txt`: View the contents of flag2.txt
- **Final Flag:** `THM{c8eeb0468febbadea859baeb33b2541b}`



---
<img width="621" height="479" alt="Screenshot 2026-04-29 at 8 11 28 PM" src="https://github.com/user-attachments/assets/68372b04-fd7a-43a3-84df-b83280bc74af" />

---

<img width="479" height="332" alt="Screenshot 2026-04-29 at 8 11 13 PM" src="https://github.com/user-attachments/assets/463aed88-ad67-4391-be61-170608c6a6a0" />

