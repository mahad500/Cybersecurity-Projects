# Incident Report – Windows Logging for SOC

### Task 1 – Event Log Review (Successful Login Identification)

**Screenshot:** `screenshots/01-successful-logon-4624.jpg`

![Successful Login](screenshots/01-successful-logon-4624.jpg)

**Description / Analysis:**
- The screenshot shows Windows Event Viewer with the Security log selected and Event ID 4624 highlighted.
- Event ID 4624 represents a successful user authentication event used to track legitimate and suspicious logins.
- No abnormal behavior is observed at this stage, but this establishes a baseline for further investigation.

### Task 2 – Brute Force and Malicious RDP Detection (Event IDs 4624 & 4625)

**Screenshot:** `screenshots/02-bruteforce-rdp-main.jpg`

![Brute Force and RDP Analysis](screenshots/02-bruteforce-rdp-main.jpg)

**Description / Analysis:**
- The screenshot shows Windows Event Viewer filtered for Security logs highlighting Event ID 4624 (successful login) for the Administrator account.
- The successful RDP login (Logon Type 10) is linked to Logon ID **0x183C36D**, confirming unauthorized access.
- Prior to the successful login, multiple failed login attempts (Event ID 4625) from IP **10.10.53.248** indicate a brute force attack.
- The Administrator account was breached as a result of these attempts.
- By referencing both failed and successful login events in the analysis, we demonstrate the ability to correlate log data and identify the sequence of malicious activity without needing multiple screenshots.

### Task 3 – User Management Events and Backdoor Account Detection

**Screenshot:** `screenshots/03-backdoor-user.jpg`

![Backdoor User Detection](screenshots/03-backdoor-user.jpg)

**Description / Analysis:**
- The screenshot shows Windows Event Viewer displaying a **4720 (user account created)** event for the backdoor account **svc_sysrestore**.
- This account was created immediately after the successful RDP login from Task 2, as indicated by the matching **Logon ID**.
- Additional events show that the backdoor user was added to the privileged groups: **Backup Operators** and **Remote Desktop Users**, granting it administrative and remote access capabilities.
- This demonstrates that the attacker created a persistent backdoor account and assigned it privileges to maintain control over the system.
- Correlating the Logon ID from the previous task confirms that the account creation is linked directly to the malicious session.

### Task 4 – Malicious File Download and Execution (Sysmon Analysis)

**Screenshot:** `screenshots/04-sysmon-malware-execution.jpg`

![Sysmon Malware Execution](screenshots/04-sysmon-malware-execution.jpg)

**Description / Analysis:**
- The screenshot shows a **Sysmon Event ID 1 (Process Creation)** log for the execution of **ckjg.exe**.
- The process was launched from the directory:  
  `C:\Users\sarah.miller\Downloads\ckjg.exe`, indicating it was downloaded from the internet.
- The parent process and command line fields indicate that the file was opened through **Google Chrome**, confirming the browser used.
- Additional Sysmon network events revealed that the file was downloaded from:  
  `http://gettsveriff.com/bgj3/ckjg.exe`.
- The Logon ID matches Sarah’s session, linking this activity directly to the compromised user account.
- This confirms that the attacker gained access by tricking the user into downloading and executing a malicious file.
- This activity likely enabled further lateral movement and brute-force attacks against production servers.








