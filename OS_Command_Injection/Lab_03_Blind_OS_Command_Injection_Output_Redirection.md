# Lab 03: Blind OS Command Injection with Output Redirection

### 🎯 Objective
Exploit a blind OS command injection vulnerability by redirecting the command output to a writable directory and retrieving the result via a static file request.

### 🔍 Vulnerability Analysis
The application's feedback function is vulnerable to blind OS command injection. Since the output isn't reflected in the response, we leverage the fact that `/var/www/images/` is a writable directory used for serving product images. By redirecting the output of our command into a text file in this directory, we can later access it through the image loading functionality.



### 🛠️ Exploitation Steps

1.  **Command Injection & Redirection:**
    I intercepted the feedback submission request (`POST /submit-feedback`) and injected the following payload into the `email` parameter:
    
    `email=||whoami>/var/www/images/output.txt||`
    
    * `||`: Chaines the commands.
    * `whoami`: The command to identify the current user.
    * `>`: Redirects the standard output of `whoami` to a file.
    * `/var/www/images/output.txt`: The writable path where the result is saved.

2.  **Retrieving the Output:**
    The application loads images using a request like `GET /image?filename=something.jpg`. I intercepted a request to load an image and changed the `filename` parameter to point to my created file:
    
    `GET /image?filename=output.txt`

3.  **Verification:**
    The server responded with the content of `output.txt`, revealing the name of the system user.

### 🚩 Result
- **Captured User:** `peter-XXXXXXXX`
- **Evidence:** Accessed the command output through the image retrieval endpoint.
- **Status:** ✅ Solved
