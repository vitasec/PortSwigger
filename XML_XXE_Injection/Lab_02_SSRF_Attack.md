# Lab 02: Exploiting XXE to perform SSRF attacks

### 🎯 Objective
Exploit an XXE vulnerability to perform an SSRF attack against a simulated EC2 metadata endpoint (`http://169.254.169.254/`) and retrieve the **IAM secret access key**.

### 🔍 Background
The application's XML parser is vulnerable to external entities. Since the server is hosted on an AWS-like environment, we can point an entity to the internal metadata URL to extract sensitive cloud credentials.

### 🛠️ Exploitation Process

1. **Initial Vector:**
   Capture the "Check stock" request. It sends XML like this:
   ```xml
   <stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
