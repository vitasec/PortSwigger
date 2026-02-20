# Lab 08: Exploiting XXE via Image Upload (SVG)

### 🎯 Objective
Exfiltrate the contents of the `/etc/hostname` file by uploading a malicious SVG image that triggers an XXE vulnerability during server-side image processing.

### 🔍 Methodology: SVG-Based XXE
SVG (Scalable Vector Graphics) is an XML-based image format. If an application allows SVG uploads and uses a library like Apache Batik to process them, an attacker can define an external entity within the SVG. When the server renders the image, it resolves the entity and embeds the result (e.g., file content) directly into the generated graphic.



### 🛠️ Exploitation Process

1. **Creating the Malicious SVG:**
   I created an SVG file containing a DTD that defines an external entity to read the server's hostname. The `<text>` element is then used to display the value of that entity.

   **Payload:**
   ```xml
   <?xml version="1.0" standalone="yes"?>
   <!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
   <svg width="128px" height="128px" xmlns="[http://www.w3.org/2000/svg]" xmlns:xlink="[http://www.w3.org/1999/xlink]" version="1.1">
     <text font-size="16" x="0" y="16">&xxe;</text>
 

    Uploading the Payload:

        I navigated to a blog post and opened the comment section.

        I filled in the required comment details and uploaded the SVG file as an avatar.

    Retrieving the Flag:

        After submitting the comment, I viewed the post.

        The avatar image was processed by the server, and the text inside the image displayed the content of /etc/hostname.

🚩 Result

    Captured Hostname: [REDACTED]

    Evidence: The hostname was visually rendered inside the avatar image on the blog page.

    Status: ✅ Solved
