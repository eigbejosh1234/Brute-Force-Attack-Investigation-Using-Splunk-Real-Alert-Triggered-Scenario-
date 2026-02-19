# Brute-Force-Attack-Investigation-Using-Splunk-Real-Alert-Triggered-Scenario-
This project documents a real-world brute force login investigation triggered by a Splunk alert. I analyzed windows Security Event Logs (EventID 4625 &amp; 4624) to identify failed login attempts, credential guessing patterns, and eventually successful access.


**The investigation includes:** <br>
* Alert review and validation <br>
* Log analysis using SPL queries. <br>
* Identification of source IP and targeted account. <br>
* Timeline reconstruction. <br>
* Detection of failed-to-successful login patterns. <br>
* SOC-style incident documentation.

  This project simulates a real Security OPerations Center (SOC) workflow for detecting and investigating brute force attacks in a controlled lab environment.

  **STEP1 VIEWING OF ALERT DASHBOARD** <br>
    I clicked on **Alert** to view if an alert has been triggered. So i discovered that an alert titled **BRUTE FORCE DETECTION LAB VALIDATED** was already triggered.

<img width="561" height="139" alt="image" src="https://github.com/user-attachments/assets/81d936c5-b709-4ba4-ab3b-2cc1a15c2c4a" />


I clicked on the title for proper investigation and i discovered that two result has already been recorded with timestamp

**(1)** 	2026-02-19 11:05:00 WAT 	View Results <br>
**(2)** 	2026-02-18 14:10:01 WAT 	View Results

   
  I clicked on the recent time alert **2026-02-19 11:00** to view the result for further investigation.

<img width="561" height="200" alt="image" src="https://github.com/user-attachments/assets/f2e2a51b-be75-4302-a831-da950492203b" />

From the image pasted above, you discovered there are two statistc recorded. And from the statistcis, the below were recorded: <br>
Tme <br>
Account_Name <br>
Host <br>
Count <br>

To investigate further and deeper, i clicked on **Event (6)** the first button before statistic and patterns to view all the 6 failed login attempt

<img width="584" height="250" alt="image" src="https://github.com/user-attachments/assets/39932412-710a-435a-9808-32a5a8378504" />

<img width="564" height="194" alt="image" src="https://github.com/user-attachments/assets/7dd3a7bd-1170-4686-9b21-88ad3a2d4370" />

To go further and deeper in investigation, i clicked on the **show all 61 lines** from the first event.

<img width="568" height="277" alt="image" src="https://github.com/user-attachments/assets/3b6f5a11-e028-44bd-9d70-d22fbe6413fa" />

<img width="580" height="258" alt="image" src="https://github.com/user-attachments/assets/f1e1d7d5-5e1b-47e3-b613-f6d51a86877d" />

Now i analyse further: <br>
From the above info, i discovered: <br>
EventCode=4625--------------- Failed login <br>
Logon_Type------------------- 2 <br>
Source Network Address:------	127.0.0.1 <br>
Account Name:--------------		USER <br>
Account Domain:-----------		MASTERJOETECH <br>


From the info above both the image screenshot and the one i typedout, stated that: <br>
**1** EventCode=4625: This is a failed logon.

**2** Logon_Type 2: This means someone triedd to login directly on the computer. eg typing a username and password on the login screen. Not remote nor web entry. 

**3** Account Domain: This means the failed logon was attempted on a local computer **(MASTERJOETECH)**

**4** Source_Network_ Address: This is where the login came from. **127.0.0.1 local host.**

**5** Account Name---USER: This means the logon attempt was done on the user computer **MASTERJOETECH**

From the investigation gathered, i discovered that the logon failure was done on my window os which i named MASTERJOETECH. Further more, the source IP that performed the failed login was my local host **127.0.0.1.** Because i typed a wrong password.  **Logon_type 2.**

I investigated the 6 failed logon and i discovered that the results were all the same.

**Conclussion** <br>
The the USER which is me at timesamp 02/19/2026 11.04.08.173am to 02/19/2026 11:04:26.303AM i tried to login into computer named MASTERJOETECH 6 times couldnt get access due to **REASONS** wrong password.

**TO SOLIDIFY MY INVESTIGATIONS** <br>
I need to confirm if there was a successful login after the 6 attempted failed logons. On my search box i run the below: <br>
index=* EventCode=4624 Account_Name="USER"

<img width="584" height="285" alt="image" src="https://github.com/user-attachments/assets/8576bf13-7554-4d2d-8ce9-3c7920c76c91" />

 From the above screenshot info, i discovered: <br>
EventCode=4624--------------- Successful login <br>
Logon_Type------------------- 2 <br>
Source Network Address:------	127.0.0.1 <br>
Account Name:--------------		USER <br>
Account Domain:-----------		MASTERJOETECH <br>

This confirmed my investigation to be accurate because after the timesamp02/19/2026 11:04:26.303AM to be 4625=failed, successful login4624 follows. Timestamp 2/19/26 11:05:48.665 AM. <br>
I investigated the successful login info and i discovered that the info are similar to eachother.

**Final Report**
The brute force detection alert triggered due to multiple failed login attempts (Event ID 4625) against the account USER on host MASTERJOETECH. <br>
Investigation revealed that:

6 failed login attempts occurred. <br>
A successful login (Event ID 4624) followed shortly after. <br>
The login originated from the local machine (127.0.0.1). <br>
Logon Type was 2 (interactive/local logon). <br>
No evidence of remote access or external malicious activity was identified. <br>
Risk level assessed as Low.

**Key Observations:** <br>
* All activity originated from localhost (127.0.0.1). <br>
* Logon Type 2 confirms interactive login. <br>
* No Logon Type 3 (network) or 10 (RDP) observed. <br>
* Pattern suggests user password entry errors or controlled lab simulation.

**There is no indication of:**
* External brute force attempts <br>
* Remote Desktop compromise
*Credential spraying from network sources
*Lateral movement activity

**Risk Assessment** <br>
Category	                  Assessment <br>
External Threat	            No <br>
Remote Access	              No <br>
Privilege Escalation	      No <br>
Account Compromise	        No evidence <br>
Overall Severity	          Low <br
                                  
**VERDICT**
FALSE POSITIVE
