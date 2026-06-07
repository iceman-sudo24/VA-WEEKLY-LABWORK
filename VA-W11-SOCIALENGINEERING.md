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

**TASK 1 QUESTION: Who is listed as the Merchant in the email body?**
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

**Hyperlink Tracking:**
- Source of the email message contained a Tracking.png image file which would send
  back information to the spammers server 
- Specifically tracking pixels which are very small images, were embedded in the email
- Yahoo blocked these fortunately however in the email example

---

**TASK 3 QUESTION: What root domain does the hyperlink in the above example point to? Be sure to defang the URL.**
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

**Clicking The Button:**
- User redirected to landing page that mimics OneDrive share
- Interacting with the buttons redirects the user to a fake OneDrive site
- Many critical red flags are obvious and clear

**Logging In:**
- Logging into the page just produces an error
- The real purpose is sending the victims login back to the attacker's server

---

**TASK 4 QUESTION: The attacker deployed a fake portal to capture and exfiltrate user credentials. What is this type of attack called?**
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

Email Body and Attachment Analysis:
- Suspicious PDF file attached
- Attachment contains embedded link titled "Update Payment Account"

---

**TASK 5 QUESTION: What is the actual sender email address hidden behind the Netllx billing display name?**
- ANSWER: z99@musacombi.online
- <img width="319" height="87" alt="image" src="https://github.com/user-attachments/assets/e8cb90d6-c171-4698-aa66-0cb0ad92ab1e" />


## Task 6: Your Recent Purchase

## Task 7: Scheduled Shipment


































---

# Second Exercise: ParrotPost: Phishing Analysis (TryHackMe)
