# Lab 02: Blind OS Command Injection with Time Delays

### 🎯 Objective
Exploit a blind OS command injection vulnerability in the feedback function to cause a 10-second time delay, proving the vulnerability exists even when no output is returned.

### 🔍 Vulnerability Analysis
The application processes feedback submissions and passes user-supplied fields (like email) to a shell command. Unlike the first lab, this is a **Blind** injection, meaning the application does not return the output of the command in its response. To confirm execution, we must use a command that has a measurable side effect, such as a time delay.



### 🛠️ Exploitation Steps

1.  **Identify the Injection Point:**
    I intercepted the feedback submission request (`POST /submit-feedback`). The parameters are `name`, `email`, `subject`, and `message`.

2.  **Construct the Payload:**
    I used the `ping` command to create a delay. By pinging the loopback address (`127.0.0.1`) 10 times, I can force the server to wait for approximately 10 seconds before responding.
    
    **Payload injected into the `email` parameter:**
    `email=x||ping+-c+10+127.0.0.1||`
    
    * `x`: A dummy value for the email field.
    * `||`: Shell operator that executes the next command if the previous one fails (or just to chain them).
    * `ping -c 10 127.0.0.1`: Command to send 10 ICMP packets, causing a ~10-second delay.

3.  **Verify Execution:**
    Upon sending the modified request, I observed the **Response Time** in Burp Suite. The server took exactly **10,145 millis (10.1s)** to respond, confirming that the command was successfully executed.

### 🚩 Result
- **Evidence:** Measured response delay of 10 seconds.
- **Status:** ✅ Solved
