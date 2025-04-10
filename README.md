# Network Protocol Vulnerability Exploration Lab

## A. Objective

The goal of this lab is to explore the vulnerabilities of common network protocols (FTP, TELNET, SSH, HTTP) by performing brute force attacks to recover passwords and then using those credentials to sniff network traffic. You will also analyze the security of these protocols and propose mitigation strategies.

---

## B. Lab Tasks

### 1. Enumerate the Vulnerable VM to Discover Usernames
- **Task**: Identify potential usernames for brute force attacks.
- **Documentation**: Record the usernames you find during enumeration.

### 2. Perform Brute Force Attacks

#### 2.1 FTP, TELNET, and SSH
- **Tools**: Use tools like Hydra, Medusa, or NetExec for brute force attacks against the following protocols:
  - **FTP**
  - **TELNET**
  - **SSH**

  Example Hydra commands:
  - **FTP**: 
    ```bash
    hydra -l <username> -P <password_list.txt> ftp://<target_ip>
    ```
  - **TELNET**:
    ```bash
    hydra -l <username> -P <password_list.txt> telnet://<target_ip>
    ```
  - **SSH**:
    ```bash
    hydra -l <username> -P <password_list.txt> ssh://<target_ip>
    ```

#### 2.2 HTTP
- **Tool**: Use Burp Intruder to automate brute force attacks against an HTTP login page.
  - Configure Intruder to test a list of usernames and passwords.
  - Analyze results to identify successful logins.

### 3. Sniff Network Traffic
- **Action**: Use the recovered credentials to log in to the respective services.
- **Tools**: Use Wireshark or tcpdump to capture and analyze network traffic during the session.
  - Identify which protocols transmit data in plaintext and which use encryption.
  - Provide evidence (screenshots) showing which protocols are secure and which are not.

### 4. Analyze Problems Encountered
- **Task**: Document any issues during brute force attacks (e.g., rate limiting, protocol-specific challenges).
- **Solution**: Explain how these issues were resolved, e.g., using proxy chains, adjusting request rate, or choosing different wordlists.

### 5. Propose Mitigation Strategies
For each vulnerable protocol, propose a secure alternative and explain how these alternatives mitigate the vulnerabilities:
- **FTP**: Use SFTP (Secure File Transfer Protocol).
- **TELNET**: Switch to SSH for encrypted communication.
- **SSH**: Use public key authentication and disable password-based login.
- **HTTP**: Use HTTPS for encrypted communication between client and server.

### 6. Write a Walkthrough
- **Document your entire process in a walkthrough format, including**:
  - Tools used.
  - Commands executed.
  - Screenshots of key steps.
  - Analysis of results.
  - Mitigation strategies.

---

## C. Deliverables

### 1. Walkthrough Document
- A detailed markdown file documenting your process, results, and analysis.

### 2. Evidence
- Screenshots of brute force attacks, packet captures, and successful logins.

### 3. GitHub Repository
- Push your walkthrough and evidence to your public GitHub repository.

### 4. Demo and Debrief
- A 5-15 minute live demo of your lab work, including:
  - Explanation of tools and techniques used.
  - Demonstration of brute force attack and sniffing process.
  - Summary of findings and mitigation strategies.
- The debrief will assess your understanding of the topic while enhancing your public speaking and presentation skills.

---

## D. Demo and Debrief
- **Objective**: Present the tools used, explain the attack methods, demonstrate the brute force attack, show the sniffed traffic, and discuss mitigation strategies for each protocol.
  - **Structure**:
    - Quick overview of tools.
    - Brute force demonstration on FTP, TELNET, SSH, and HTTP.
    - Summary of findings (e.g., FTP is vulnerable to password sniffing).
    - Explanation of mitigation strategies (e.g., replace FTP with SFTP).

---

## E. Submission Instructions

1. **Create a GitHub repository** and upload your walkthrough in Markdown format, including the evidence (screenshots).
2. **Submit the repository link** to your instructor for review.
3. **Prepare for the live demo**: Be ready to present and answer any questions during the lab session.

---

## Tools Used

- Hydra
- Medusa
- NetExec
- Burp Suite
- Wireshark
- tcpdump

---

## References
- [Hydra - Brute Force Tool](https://github.com/vanhauser-thc/thc-hydra)
- [Burp Suite - Intruder](https://portswigger.net/burp)
- [Wireshark](https://www.wireshark.org/)
