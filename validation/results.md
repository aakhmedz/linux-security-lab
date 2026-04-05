## Validation — Security Control Effectiveness

### Objective

The purpose of this phase is to validate that implemented security controls effectively detect and prevent previously observed attack behavior.

This confirms that the system has transitioned from a vulnerable state to a hardened and monitored environment.

---

### Step 1 — Re-run Brute Force Attack

From the attacker system (Kali Linux), execute the same brute-force attack used earlier:

hydra -l secureuser -P rockyou.txt ssh://<target-ip> 

This replicates the original attack conditions.

---

### Step 2 — Observe Attack Behavior

During execution, monitor the attack output.

Expected behavior:

- Initial failed login attempts 
- Sudden interruption of attack progress 
- Connection failures after multiple retries 

Example output:

![Hydra Blocked](../screenshots/day5/hydra-blocked.png)

---

### Step 3 — Verify Fail2Ban Response

Check Fail2Ban status to confirm the attacking IP has been banned:

sudo fail2ban-client status sshd 

Expected result:

- Attacker IP listed as banned 
- Increased ban count 

Example output:

![Fail2Ban Banned IP Confirmation](../screenshots/day3/fail2ban-status.png)

---

### Step 4 — Confirm Log Evidence

Review authentication logs:

sudo tail -f /var/log/auth.log 

Look for:

- Multiple failed login attempts 
- Ban action triggered 
- Blocked connection attempts 

Example evidence:

![Authentication Logs Showing Blocked Behavior](../screenshots/day5/auth-blocked.png)

---

### Step 5 — Validate Persistence Detection

Check for unauthorized scheduled tasks:

crontab -l 

Inspect running processes:

ps aux 

Verify that persistence mechanisms are:

- Detected 
- Identified 
- Removable 

---

### Step 6 — Validate File Integrity Monitoring

Run AIDE integrity check:

sudo aide --check 

Expected result:

- Detection of unauthorized file changes 
- Integrity violations reported 

Example output:

![AIDE Detection](../screenshots/day4/aide-alert.png)

---

### Results Summary

Control                     Result
--------------------------  ----------------------------
Brute force protection      Attack blocked
IP banning                  Attacker IP denied access
Log visibility              Full detection of activity
Persistence detection       Identified via cron/process
File integrity monitoring   Unauthorized changes detected

---

### Security Improvements

Before implementation:

- No protection against brute-force attacks  
- No automated detection or response  
- No file integrity monitoring  
- No persistence visibility  

After implementation:

- Automated IP banning via Fail2Ban  
- Real-time log monitoring  
- File integrity monitoring with AIDE  
- Ability to detect persistence mechanisms  

---

### Conclusion

The validation phase confirms that implemented security controls are functioning as intended.

The system is now capable of:

- Detecting malicious authentication attempts  
- Automatically blocking attackers  
- Identifying persistence techniques  
- Monitoring integrity of critical system files  

This demonstrates a successful transition from a vulnerable system to a monitored and defensible environment.
