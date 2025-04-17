# 🛡️ Network Protocol Vulnerability Lab – Walkthrough

## 📌 Objective  
Simulate brute-force attacks on common network services (**FTP**, **TELNET**, **SSH**, **HTTP**), assess their security posture, capture and analyze network traffic, and recommend mitigations.

---

## 🖥️ Lab Environment

| Component     | Details                               |
|---------------|---------------------------------------|
| **Attacker VM** | Kali Linux 2024.4                     |
| **Target VM**   | Metasploitable2 / Custom Vulnerable Linux |
| **Tools Used**  | Hydra, Burp Suite, Wireshark          |

---

# 🧾 Task 1: Enumeration of Target

### 🎯 Goal  
Discover valid usernames and running services on the target system.

---

### 🔍 Step 1.1 – Perform Nmap Scan

**Command Used:**
```bash
nmap -p20,21,22,23,80 <target-ip>
```
![Screenshot 2025-04-17 214346](https://github.com/user-attachments/assets/1cd0a0a5-c008-4874-ab17-3a670d8e2cc9)

---

## 1.2 🧰 Enum4linux Enumeration

Since common ports such as **FTP (21)**, **TELNET (23)**, **SSH (22)**, and **HTTP (80)** are open on the target, we used `enum4linux` to perform enumeration and attempt to extract valid usernames and other valuable SMB-related information.

### 🔧 Command Used:
```bash
enum4linux -a <target-ip>
```
![Screenshot 2025-04-17 214452](https://github.com/user-attachments/assets/15e1e011-11cd-4606-abe0-fdfd75ec00da)
---

# 🔐 Task 2: Brute Force Attacks

## ✅ Preparation

Before initiating brute-force attacks, prepare the required wordlists:

- `userlist.txt` – A list of potential usernames
- `passlist.txt` – A list of potential passwords

### 🗂️ Wordlist Options

You can choose to:

- Use built-in wordlists provided by Kali Linux (e.g., `/usr/share/wordlists/rockyou.txt`)
- Or create simple custom wordlists for testing:

```bash
# Create a username list
echo -e "admin\nmsfadmin\nanonymous\nuser\ntest" > userlist.txt

# Create a password list
echo -e "1234\nmsfadmin\nftp123\nadmin\npassword" > passlist.txt
```

## 🔹 2.1 FTP Brute Force with Hydra

We used **Hydra** to perform a brute-force attack on the FTP service running on the target.

### 🔧 Command Used:
```bash
hydra -L userlist.txt -P passlist.txt ftp://<TARGET_IP> -V
```
![Screenshot 2025-04-17 213621](https://github.com/user-attachments/assets/a09f7343-70f4-4e46-9bfd-eb8651ab1879)
---

## 🔹 2.2 Telnet Brute Force with Hydra
Command to attack Telnet:
```bash
hydra -L userlist.txt -P passlist.txt telnet://<TARGET_IP> -V
```
![Screenshot 2025-04-17 213857](https://github.com/user-attachments/assets/45a06840-0981-4764-a5f7-b0749953d013)
---

## 🔹 2.3 SSH Brute Force with NetExec
Command to attack SSH:
```bash
nxc ssh <TARGET_IP> -u userlist.txt -p passlist.txt
```
![Screenshot 2025-04-17 214121](https://github.com/user-attachments/assets/aa80c2d9-b489-479f-a9d1-299c67f7108a)

---

## 🔹 2.4 HTTP Login Brute Force Using Burp Suite Intruder

### Step 1: Launch Burp’s Embedded Browser
- Open **Burp Suite**.
- Navigate to `Proxy > Intercept`.
- Ensure **Intercept is ON**.
- Click **Open Browser** to launch Burp’s embedded browser.

### Step 2: Access the Target (Metasploitable2)
- Open a Firefox browser (or use Burp's browser).
- Enter the target IP address (e.g., `http://<TARGET_IP>`).
- Navigate to the **DVWA** section.
- Login using:
  - **Username:** `admin`
  - **Password:** `password`
- In the left panel, click on **Brute Force**.
- Enter any sample values into the username and password fields (e.g., `aaa` for both), then click **Login**.

### Step 3: Capture and Forward the Request
- In Burp’s `Proxy > Intercept` tab, the login request will be captured.
- Click **Forward** to send the request to the server.
- Continue clicking **Forward** if multiple requests are captured until the page finishes loading.
- Go to the `Proxy > HTTP history` tab.
- Locate the request to `/dvwa/vulnerabilities/brute/?username=aaa&password=aaa&Login=Login`.
- Right-click the request and select **Send to Intruder**.

### Step 4: Turn Off Interception
- Return to `Proxy > Intercept` and click **Intercept is OFF** to stop intercepting further requests.

### Step 5: Configure the Intruder Attack
- Go to the **Intruder** tab.
- Set the **Attack Type** to **Cluster Bomb**.
- Highlight the username and password values in the request and mark them as payload positions.
- In the **Payloads** tab:
  - For Position 1 (Username), load a **username list** (e.g., `userlist.txt`).
  - For Position 2 (Password), load a **password list** (e.g., `passlist.txt`).
  - These files can typically be found in `/usr/share/wordlists`.

### Step 6: Launch the Attack
- Click **Start Attack**.
- Monitor the results by checking the **Response** and **Length** columns.
- Look for responses that differ in length or content.
- You can also click **Render** to view the visual output of each response.
- A successful login might return a message like:  
  **"Welcome to the password protected area admin"**
- Failed attempts usually return:  
  **"Username and/or password incorrect"**


