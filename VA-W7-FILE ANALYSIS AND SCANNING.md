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
### What vulnerabilities are likely present based on the version?
### Which one is the highest risk and why?
### What attack path can be built from this?
### What should be the remediation?

---

## QUESTION 4: IDENTIFY THE OS (OS Fingerprinting) - TTL

### IMAGE 1
<img width="504" height="176" alt="Screenshot 2026-04-30 at 8 11 54 PM" src="https://github.com/user-attachments/assets/914885a3-cdd2-4c48-b938-20fe660acb3f" />

**ANSWER:**

---

### IMAGE 2
<img width="281" height="194" alt="Screenshot 2026-04-30 at 8 12 18 PM" src="https://github.com/user-attachments/assets/72bca447-b27a-4a49-b134-55eca046fd28" />

**ANSWER:**

---

### IMAGE 3
<img width="516" height="116" alt="Screenshot 2026-04-30 at 8 12 45 PM" src="https://github.com/user-attachments/assets/c0a81c1c-01db-44da-8a71-5491c9096b1f" />

**ANSWER:**

---



---

## QUESTION 5: ANALYSE THE NESSUS FILE
Upload to your nessus (Network_Scan.nessus) and analyse the files. Focus on critical or high findings that was identifies in analysis named “Ghostcat”.

### What is the affected Port number?
### What is the Affected protocol?
### What is the CVSS Score of vulnerability found?
### Can you find any exploit related to this vulnerability?
### Find CVE for this vulnerability.

