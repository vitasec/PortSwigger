# Lab 07: Exploiting XXE via XInclude Attack

### 🎯 Objective
Retrieve the contents of the `/etc/passwd` file in a scenario where the attacker does not control the entire XML document and cannot define a DTD.

### 🔍 Methodology: XInclude Injection
When you can only control a single value (like a parameter) that is placed into a server-side XML template, classic XXE is impossible. However, if the XML parser supports **XInclude**, we can inject a specific XML namespace that allows us to reference and include external files.



### 🛠️ Exploitation Process

1. **Identifying the Constraint:**
   The `productId` is sent as a regular POST parameter, but it's parsed as part of an XML document on the server. Attempting to inject `<!DOCTYPE...` fails because the input is placed inside an existing element.

2. **The XInclude Payload:**
   We use the `XInclude` namespace to define a reference to a local file.

   **Payload to inject into `productId`:**
   ```xml
   <foo xmlns:xi="[http://www.w3.org/2001/XInclude]">
       <xi:include parse="text" href="file:///etc/passwd"/>
   </foo>

    Breakdown of the Payload:

        xmlns:xi="http://www.w3.org/2001/XInclude": Defines the XInclude namespace.

        xi:include: The command to include external content.

        parse="text": Tells the parser to treat the file content as plain text, not XML (prevents parsing errors).

        href="file:///etc/passwd": The path to the sensitive file.

    Execution:

        Intercept the POST request.

        Replace the value of productId with the payload (ensure it is URL-encoded if necessary).

        The server processes the XML, executes the XInclude, and returns the file content in the response.

🚩 Result

    Evidence: The /etc/passwd file content appeared in the application's response.

    Status: ✅ Solved
