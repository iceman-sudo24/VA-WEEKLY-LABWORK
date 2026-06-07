# First Exercise: Phishing Emails In Action (TryHackMe)

## Task 1: Introduction
- This section summarized the entire course and provided four objectives:
  - Identify common social engineering tactics used in phishing
  - Analyze red flags contained within phishing emails
  - Detect link manipulation and tracking pixels
  - Deconstruct credential harvesting and attachment manipulation
 
This course involves examining real phishing email samples, learning the tactics
attackers use to impersonate legitimate entities. As a result, this will assist in
my understanding/skills of identifying nuances and subtle details in Phishing emails as 
well as distinguish a routine notification from a complex credential harvesting attempt.

## Task 2: Cancel Your Order
This section focused on a sample email that mimicked an official transaction receipt from PayPal to 
learn how attackers leverage spoofed email addresses, impersonate trusted services and their strategic
use of URL shortening services to hide the final destionation of malicious links.

**Techniques The Attacker Used:**
- Spoofed email address
- URL shortening

**First Observations:** 
- Attention-grabbing subject line
- Mismatched from address
- Unusual email recipient address and not a normal yahoo email domain

<img width="804" height="141" alt="image" src="https://github.com/user-attachments/assets/b58bfe69-125d-4195-b6c9-46df4676c875" />

---

**Email Body Analysis:**
- No attachments
- Only interactive element is cancel order button

**Button Investigation:**
- Leads to a shortened URL
- Final destination is obfuscated/hidden
- Rule is to never interact with (suspicious) buttons or links without confirming exactly
  where they lead or do

**Good Tools**
- `WhereGoes` is an online tool that can be used to investigate shortened URL's without
  visting the destination

---

**TASK 2 QUESTION**
1) Who is listed as the Merchant in the email body?**
- ANSWER: Amazing Stuff
- <img width="279" height="308" alt="image" src="https://github.com/user-attachments/assets/a7e39945-7341-4341-ad08-0488fe40e126" />

## Task 3: Track Your Package
This section investigates an email that mimics a formal shipping notification that tricks
receivers with a sense of urgency.

Phishing Techniques Used:
- Spoofed email address
- Pixel tracking
- Link manipulation

**First Observations:**
- Used a fake tracking number to create sense of urgency and entice the user to click to see their
package status
- The display name does not match the actual sender address
- There is a hyperlink in the email body that matches the subject line, although we do not know where it directs to just yet

<img width="797" height="170" alt="image" src="https://github.com/user-attachments/assets/ccad0833-c135-4439-9554-1c9a4e0e2aae" />

---

**Hyperlink Tracking:**
- Source of the email message contained a Tracking.png image file which would send
  back information to the spammers server 
- Specifically tracking pixels which are very small images, were embedded in the email
- Yahoo blocked these fortunately however in the email example

---

**TASK 3 QUESTION**
1) What root domain does the hyperlink in the above example point to? Be sure to defang the URL.
- ANSWER: devret[.]xyz 
- Defanging the URL is making it non-functional and safe for sharing, in this case square-bracketing the
  period character '.'

## Task 4: Download Document Here
This task involves analyzing a phishing campaign which utilizes a multi-stage redirection chain
to harvest user credentials

**Phishing Techniques Used**
- Artificial urgency
- Brand impersonation
- Link redirection: Using a chain of URLs to hide the final malicious destination from basic email filters
- Credential harvesting: Deploying a fake login portal to capture and exfiltrate usernames and passwords

**First Observations:**
- Send date
- Expiration date (for sense of urgency)
- Download Document here button (button to download a fax)

<img width="798" height="341" alt="image" src="https://github.com/user-attachments/assets/0f0c0868-10d3-4767-9ba1-d9f0e4c54050" />

---

**Clicking The Button:**
- User redirected to landing page that mimics OneDrive share
- Interacting with the buttons redirects the user to a fake OneDrive site
- Many critical red flags are obvious and clear

**Logging In:**
- Logging into the page just produces an error
- The real purpose is sending the victims login back to the attacker's server

---

**TASK 4 QUESTION**
1) The attacker deployed a fake portal to capture and exfiltrate user credentials. What is this type of attack called?
- ANSWER: Credential Harvesting

## Task 5: Your Account Is On Hold
This task involves examining an email that is impersontating a household brand that is demanding
immediate action from the recipient.

**Phishing Techniques Used**
- Spoofed email address
- Sense of urgency
- Brand impersonation
- Poor grammar and typos
- Attachments: Using a file attachment rather than a direct link to hide the malicious URL

**First Observations:**
- Receivers ID was suspended and must act quickly (allegedly, urgency tactic)
- Display name does not match user and domain
- Email uses rendered HTML to impersonate Netflx

<img width="793" height="228" alt="image" src="https://github.com/user-attachments/assets/e5676217-42cd-4a07-8912-5a1a3551c832" />

---

Email Body and Attachment Analysis:
- Suspicious PDF file attached
- Attachment contains embedded link titled "Update Payment Account"

---

**TASK 5 QUESTION**
1) What is the actual sender email address hidden behind the Netllx billing display name?
- ANSWER: z99@musacombi.online
- <img width="319" height="87" alt="image" src="https://github.com/user-attachments/assets/e8cb90d6-c171-4698-aa66-0cb0ad92ab1e" />

## Task 6: Your Recent Purchase
This task looks at and analyzes a phising attempt that disguises itself as a billing notification from a major service provider (Apple Support)

**Phishing Techniques Used**
- Spoofed email address
- Recipient is BCCed: The victim is not directly sent the email
- Urgency
- Poor grammar and typos
- Attachments: The email contains a .dot (opens in new tab) file (Microsoft Word Template), which is an unusual format for a receipt

**First Observations:**
- Recipient is told to act quickly to resolve a purchase they did not make
- Their display name does not match user and domain
- The recipient was not directly emailed but BCCed
    - a feature in email that allows you to send a copy of a message to recipients without revealing their  
    addresses to others.

<img width="795" height="122" alt="image" src="https://github.com/user-attachments/assets/f1fa490f-ccd6-4409-823d-a854002e33e7" />

---

**Analyzing Attachment**
- Email body is completely blank
- There is an attachment in the form of a `.dot` file
- Interacting with the large image redirects the recepient to a phishing site

---

**TASK 6 QUESTIONS**
1) What does the acronym BCC stand for?
- ANSWER: Blind Carbon Copy
2) What is the file extension of the attachment?
- ANSWER: `.dot`

## Task 7: Scheduled Shipment
This part looks at a phishing attempt that disguises as a global shipping notification
using spoofed addresses and branded HTML to look real

**Phishing Techniques Used**
- Spoofed email address: The sender's display name is set to DHL Express
- Brand impersonation
- Attachments: An Excel document that triggers executable code upon opening

**First Observations:**
- Email subject gives the impression that DHL will be shipping a package
- The display name does not match the user and domain
- The HTML in the email body is designed to look like it was sent from DHL
  
<img width="815" height="256" alt="image" src="https://github.com/user-attachments/assets/d925a8c0-9e0b-4464-8b30-21779b4733f6" />

---

**Email Body and Attachment:**
- The primary concern is in the attached `.xlsx` file (excel file)
- The file contains many red flags and suspicious elements

**The EXE**
- When the link in the excel document is clicked, it will try to download
and execute a malicious payload named `regasms.exe`
- If successfully executed it could create a backdoor, steal data and put ransomware
on the vicitm's device

**TASK 7 QUESTION**
1) What is the name of the executable that the Excel attachment attempts to run?
- ANSWER: `regasms.exe`

---

<img width="1267" height="876" alt="Screenshot 2026-06-07 at 8 54 27 PM" src="https://github.com/user-attachments/assets/92c0837b-ce8c-4302-8b8b-95d035976a53" />

---

# Second Exercise: ParrotPost: Phishing Analysis (TryHackMe)
This TryHackMe room looks at identifying and analyzing a malicious phishing email through visual inspection, common header inspection tools, and manual deobfuscation.

**Learning Objectives**
- Understand what email headers are and familiarize yourself with common headers.
- Utilize tools for inspecting and analyzing suspicious emails and attachments.
- Learn to recognize different obfuscation techniques employed in malicious HTML, CSS, and JavaScript code.
- Room Prerequisites

- An email file was downloaded (`.eml`) along with a attachment for the email (`.htm`) which was used through this room
- <img width="353" height="216" alt="Screenshot 2026-06-07 at 9 07 10 PM" src="https://github.com/user-attachments/assets/24287fcd-4b5b-4363-8cbb-08f49b955b1e" />


# Task 3: Email Headers



**TASK 3 QUESTIONS**
1) According to the IP address, what country is the sending email server associated with?
- ANSWER: Latvia 
2) If Paul replies to this email, which email address will his reply be sent to?
- ANSWER: `no-reply@postparr0t.thm`
3) What is the value of the custom header in the email?
- ANSWER: `THM{y0u_f0und_7h3_h34d3r}`

# Task 4: Email Attachment Analysis

**TASK 4 QUESTIONS**
1) What encoding scheme is used to obfuscate the web page contents?
- ANSWER: base64 
2) What is the built-in JavaScript function used to decode the web page before writing it to the page?
- ANSWER: `atob()`
3) After the initial base64 decoding, what is the value of the leftover base64 encoded comment?
- ANSWER: `THM{d0ubl3_3nc0d3d}`

# Task 5: HTML Obsfucation

**TASK 5 QUESTIONS**
1) After decoding the HTML Entity characters, what is the text inside of the (<h1>) tag?
- ANSWER: ParrotPost Secure Webmail Login

# Task 6: CSS Obsfucation 

**TASK 6 QUESTIONS**
1) What is the reverse of CSS Minify?
- ANSWER: CSS Beautify

# Task 7: JavaScript Obsfucation 

**TASK 7 QUESTIONS**
1) What is the URL that receives the login request when the login form is submitted?
- ANSWER: `http://evilparrot.thm:8080/cred-capture.php`
2) What is the JavaScript property that can redirect the browser to a new URL?
- ANSWER: `window.location.href`

# Task 8: Putting It All Together

**TASK 7 QUESTIONS**
1) What is the flag you receive after sending fake credentials to the /cred-capture.php endpoint?
- ANSWER: `THM{c4p7ur3d_y0ur_cr3d5}`
2) What is the path on the web server hosting the log of captured credentials?
- ANSWER: `/creds.txt`
3) Based on the log, what is Chris Smith's password?
- ANSWER: `FlyL1ke!A~Bird`

---

<img width="1242" height="930" alt="Screenshot 2026-06-07 at 8 53 59 PM" src="https://github.com/user-attachments/assets/a975ac09-0a5d-481a-9304-5626f328b75d" />

---
