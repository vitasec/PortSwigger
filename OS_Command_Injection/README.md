# 🐚 OS Command Injection Labs

This directory contains my solutions and methodology for the **OS Command Injection** labs from the PortSwigger Web Security Academy.

## 📖 Overview

OS Command Injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running an application. This typically leads to full compromise of the application and its data.



## 🛠️ Techniques Covered

Throughout these labs, I have practiced several exploitation techniques:

* **Simple Injection:** Direct output reflection in the response.
* **Blind Injection (Time Delays):** Using time-based side effects (`sleep`, `ping`) to confirm execution.
* **Blind Injection (Output Redirection):** Writing command results to writable static directories (e.g., `/var/www/images/`).
* **Out-of-Band (OOB) Interaction:** Triggering DNS/HTTP lookups to external servers (Burp Collaborator).
* **OOB Data Exfiltration:** Stealing sensitive data by nesting commands inside OOB requests.

## 🧪 Lab Solutions

| Lab # | Title | Difficulty | Focus |
| :--- | :--- | :--- | :--- |
| 01 | [Simple Command Injection](./Lab_01_Simple_Command_Injection.md) | Apprentice | Direct Reflection |
| 02 | [Blind Injection - Time Delays](./Lab_02_Blind_Command_Injection_Time_Delays.md) | Practitioner | `ping` for latency |
| 03 | [Blind Injection - Output Redirection](./Lab_03_Blind_OS_Command_Injection_Output_Redirection.md) | Practitioner | File write & retrieval |
| 04 | [Blind Injection - OOB Interaction](./Lab_04_Blind_OS_Command_Injection_OOB.md) | Practitioner | DNS Lookups |
| 05 | [Blind Injection - Data Exfiltration](./Lab_05_Blind_OS_Command_Injection_Data_Exfiltration.md) | Practitioner | Subdomain exfiltration |

## 🚀 Key Payloads Used

* **Concatenation:** `&`, `&&`, `;`, `|`, `||`
* **Exfiltration:** `` `whoami` ``, `$(id)`
* **Redirection:** `>` , `>>`

---
*Note: These labs were completed as part of my journey to master web application security and penetration testing.*
