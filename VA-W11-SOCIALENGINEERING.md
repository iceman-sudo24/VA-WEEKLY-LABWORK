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

**Tools Used**
- KaliLinux VM (to do the room)
- KaliLinux Text Editor
- IPLocator (iplocation.net)
- MXToolBox Email Analyzer
- CyberChef Decoder
- Website Inspector


**Learning Objectives**
- Understand what email headers are and familiarize yourself with common headers.
- Utilize tools for inspecting and analyzing suspicious emails and attachments.
- Learn to recognize different obfuscation techniques employed in malicious HTML, CSS, and JavaScript code.
- Room Prerequisites

- An email file was downloaded (`.eml`) along with a attachment for the email (`.htm`) which was used through this room
<img width="353" height="216" alt="Screenshot 2026-06-07 at 9 07 10 PM" src="https://github.com/user-attachments/assets/24287fcd-4b5b-4363-8cbb-08f49b955b1e" />

---

# Task 3: Email Headers
This task focuses on analyzing the metadata of the suspicious `.eml` file (URGENTParrotPostAccountUpdateRequired.eml) using a text editor in the KaliVM. We open the suspicious .eml file (URGENTParrotPostAccountUpdateRequired.eml) using the KaliLinux (mousepad) text editor and copy the following contents into an online header analyzer like MXToolbox to decode it in Base64.

---

<img width="1280" height="720" alt="Screenshot 2026-06-07 at 8 19 06 PM" src="https://github.com/user-attachments/assets/6a440a27-3817-4ff9-a4dc-427ba31d7d4b" />
<img width="1645" height="392" alt="Screenshot 2026-06-07 at 7 46 02 PM" src="https://github.com/user-attachments/assets/6d0fa438-6bb0-488d-8f8d-511b35980af4" />
<img width="577" height="481" alt="Screenshot 2026-06-07 at 7 45 02 PM" src="https://github.com/user-attachments/assets/92d161dc-3645-430b-8384-6b17e02c9f16" />

---

- Step 1: Scan the headers for the From: line. Attackers frequently fake ("spoof") this to look like a trusted company.
- Step 2: Look closely for a Reply-To: header. This is where the trick falls apart. Even if the email says it is from a trusted company, the Reply-To header dictates the actual destination where the victim's response will be sent.
- Step 3: Inspect the custom headers. These are extra fields starting with X- (like X-Mailer or X-Spam-Status) left behind by the mail servers or the specific software the attacker used to blast out the email.

The IP involved was located with iplocation.net where the IP was entered to find out the IP origins.

<img width="200" height="89" alt="Screenshot 2026-06-07 at 9 30 04 PM" src="https://github.com/user-attachments/assets/80e26360-bbe4-411d-8c96-987d90034845" />


---
The comprehensive results of the Email Analyzer provided the answers for Question 2 and 3 below
<img width="470" height="35" alt="Screenshot 2026-06-07 at 7 57 07 PM" src="https://github.com/user-attachments/assets/caf2176b-9ff7-4d20-a68f-d71a30505e02" />
<img width="341" height="31" alt="Screenshot 2026-06-07 at 7 45 39 PM" src="https://github.com/user-attachments/assets/637d47d8-e696-4897-9231-550c59f4746b" />

---

**TASK 3 QUESTIONS**
1) According to the IP address, what country is the sending email server associated with?
- ANSWER: Latvia 
2) If Paul replies to this email, which email address will his reply be sent to?
- ANSWER: `no-reply@postparr0t.thm`
3) What is the value of the custom header in the email?
- ANSWER: `THM{y0u_f0und_7h3_h34d3r}`

# Task 4: Email Attachment Analysis
Instead of opening the file in a browser, we open it safely inside a text editor to look at the raw source code. Right away, the web page isn't written in normal code and instead looks like a massive, scrambled block of random letters and numbers.

- Step 1: Identify the scrambling technique. The block uses a standard character set typical of Base64 encoding.
- Step 2: Look at the execution trigger. We locate a small JavaScript snippet that reads this scrambled block and passes it into a built-in browser decoding function—specifically atob()—which instantly translates the gibberish back into a live webpage.
- Step 3: Extract the hidden artifacts. By running this scrambled block through a decoding tool like CyberChef, we reveal the clean code. Hidden inside the newly revealed code, we find standard developer comments (<!-- comment -->) that contain yet another layer of encoded text to dig through.

---

<img width="1280" height="720" alt="Screenshot 2026-06-07 at 8 24 13 PM" src="https://github.com/user-attachments/assets/5a980388-d886-484e-8784-ea7b39c96fa6" />
<img width="327" height="130" alt="Screenshot 2026-06-07 at 8 24 39 PM" src="https://github.com/user-attachments/assets/e895411d-93b8-4e98-ac94-d0ccdbfbaa46" />
<img width="978" height="623" alt="Screenshot 2026-06-07 at 8 24 51 PM" src="https://github.com/user-attachments/assets/37f5f99b-f6ec-4fb3-8ea0-f2e2229f3b3b" />

---

**TASK 4 QUESTIONS**
1) What encoding scheme is used to obfuscate the web page contents?
- ANSWER: base64 
2) What is the built-in JavaScript function used to decode the web page before writing it to the page?
- ANSWER: `atob()`
3) After the initial base64 decoding, what is the value of the leftover base64 encoded comment?
- ANSWER: `THM{d0ubl3_3nc0d3d}`

# Task 5: HTML Obsfucation
Looking at the decoded HTML code from the previous step, the text still looks bizarre. The attacker didn't write normal letters; instead, they replaced standard text with strings of numbers and semicolons (e.g., &#x4c;).

- Step 1: Locate the form block stretching from the <h1> to the </form> tags.
- Step 2: Isolate the heavily obfuscated text string and paste it into CyberChef, using the "From HTML Entity" recipe.
- Step 3: Analyze the output. Once the HTML entities are converted back into plain text, look directly for the HTML <form> tag. Inside it, the action attribute explicitly names the exact destination URL where the typed credentials are sent.

---

**TASK 5 QUESTIONS**
1) After decoding the HTML Entity characters, what is the text inside of the HTML `h1` tag?
- ANSWER: ParrotPost Secure Webmail Login

<img width="252" height="73" alt="Screenshot 2026-06-07 at 8 25 31 PM" src="https://github.com/user-attachments/assets/641bb578-e37d-472d-82b1-814a6f0b1f52" />


# Task 6: CSS Obsfucation 
We shift our focus to the Cascading Style Sheets (CSS) code embedded in the document to understand how the attacker hides elements or forces user action.

- Step 1: Review the style blocks to see if certain elements are hidden or forced to take up the entire screen.
- Step 2: Track how the styling ties into background interactions. The investigation highlights client-side behaviors where specific JavaScript browser properties (like window redirection commands) are triggered to seamlessly hijack and route the user to an external site without throwing up warning flags.

---

**TASK 6 QUESTIONS**
1) What is the reverse of CSS Minify?
- ANSWER: CSS Beautify

<img width="477" height="137" alt="Screenshot 2026-06-07 at 8 26 01 PM" src="https://github.com/user-attachments/assets/f9af813d-5e06-4ddc-bd79-2c307aa17ee4" />

# Task 7: JavaScript Obsfucation 
The JavaScript code handling the login form is crammed into a dense, unreadable wall of text with no spaces, squished together onto a single line.

- Step 1: Copy the massive, single-line script block.
- Step 2: Paste it into a tool like JSBeautifier to restore standard indentation and line breaks.
- Step 3: Trace the structural flow. With the code neatly spaced out, we easily locate the primary validation function. This function sits in the middle of the workflow, checking to ensure the victim typed a real password and a properly formatted email before letting the page submit.

---

<img width="1523" height="464" alt="Screenshot 2026-06-07 at 8 32 55 PM" src="https://github.com/user-attachments/assets/172e2e3f-1908-4a2f-a50a-698afcd3559e" />
<img width="1551" height="940" alt="Screenshot 2026-06-07 at 8 33 06 PM" src="https://github.com/user-attachments/assets/768c7f1b-5937-41a5-97ea-37f658949e00" />
<img width="230" height="101" alt="Screenshot 2026-06-07 at 8 35 31 PM" src="https://github.com/user-attachments/assets/b8381455-39ab-4be0-a769-a3bf21ae965d" />
<img width="400" height="99" alt="Screenshot 2026-06-07 at 8 35 36 PM" src="https://github.com/user-attachments/assets/f988e1e3-51cf-4df1-8a10-7dd2d824ba44" />

---

**TASK 7 QUESTIONS**
1) What is the URL that receives the login request when the login form is submitted?
- ANSWER: `http://evilparrot.thm:8080/cred-capture.php`
2) What is the JavaScript property that can redirect the browser to a new URL?
- ANSWER: `window.location.href`

# Task 8: Putting It All Together
We open ParrotPostACTIONREQUIRED.htm locally in our isolated browser. Before doing anything else, we right-click and open the Developer Tools, switching directly to the Network Tab.

<img width="803" height="676" alt="Screenshot 2026-06-07 at 9 53 29 PM" src="https://github.com/user-attachments/assets/fda8dd5b-8200-4795-b10e-03d862b157ff" />

---

- Step 1: Type test data into the email and password fields and hit the login button.
<img width="364" height="319" alt="Screenshot 2026-06-07 at 9 52 44 PM" src="https://github.com/user-attachments/assets/75ee7292-afc1-421e-a8d7-e812f6b17517" />

---

- Step 2: Stop and check the Network Tab. Look for an asynchronous background data packet flying outbound. We spot a GET request hitting an endpoint named /cred-- capture.php. Inspecting the payload or response headers of this transaction drops our hidden flag.
<img width="626" height="360" alt="Screenshot 2026-06-07 at 9 55 21 PM" src="https://github.com/user-attachments/assets/c4c108a5-d786-4915-b2d6-eda24e293c11" />

---

- Step 3: Use the path found in the network traffic to navigate backward into the attacker's server directory.
<img width="731" height="324" alt="Screenshot 2026-06-07 at 9 56 00 PM" src="https://github.com/user-attachments/assets/b6ec5e7d-7f13-4d4d-a144-8345ef28a1c9" />

---

- Step 4: Open the exposed log file (the database repository where stolen data lands). Because the attacker lazily stored the stolen goods in a plain text file, we can read the harvest directly, exposing the plaintext password of an earlier victim, Chris Smith.
<img width="671" height="421" alt="Screenshot 2026-06-07 at 9 56 44 PM" src="https://github.com/user-attachments/assets/58760225-91ca-45cd-9514-788ab1e2ad59" />

---

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
