# Lab 01: OS Command Injection, Simple Case

### 🎯 Objective
Execute the `whoami` command on the target server by exploiting a vulnerability in the product stock checker feature to identify the current system user.

### 🔍 Vulnerability Analysis
The application takes user-supplied `productId` and `storeId` parameters and passes them directly into a server-side shell command to check stock levels. Since the input is not properly sanitized, an attacker can use shell metacharacters (like `|`, `;`, or `&`) to chain additional commands.



### 🛠️ Exploitation Steps

1.  **Intercept the Request:**
    I navigated to a product page and clicked the **"Check stock"** button while intercepting the request in Burp Suite.
    
    **Original Request Fragment:**
    `productId=1&storeId=1`

2.  **Inject the Command:**
    I modified the `storeId` parameter to include the `whoami` command using the pipe (`|`) operator.
    
    **Modified Payload:**
    `productId=1&storeId=1|whoami`

3.  **Analyze the Response:**
    The server executed the original stock check followed by the injected command. The response body contained the output of the `whoami` command.

### 🚩 Result
- **Captured User:** `peter-XXXXXXXX` (or the specific username returned by the lab)
- **Evidence:** The raw output of the command was reflected directly in the HTTP response.
- **Status:** ✅ Solved
