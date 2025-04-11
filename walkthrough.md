🛠️ Network Protocol Vulnerability Exploration Lab Walkthrough

Author: Wan Khairul Naim
Course: [Network Security]
Lab Title: Network Protocol Vulnerability Exploration
Date: [4/14/2025]

---

## 1. Enumerating the VM

**Goal**: Discover usernames for brute force attacks.

**Tool Used**: `enum4linux`, `nmap`, `dirb`, or manual inspection.

**Command Example**:
```bash
nmap -sV <target_ip>
enum4linux <target_ip>
```

**Discovered usernames**:
- admin
- user

---

## 2. Brute Force Attacks

### 2.1 FTP, TELNET, SSH

**Tool Used**: Hydra

#### FTP Attack
```bash
hydra -l admin -P rockyou.txt ftp://<target_ip>
```

#### TELNET Attack
```bash
hydra -l user -P rockyou.txt telnet://<target_ip>
```

#### SSH Attack
```bash
hydra -l admin -P rockyou.txt ssh://<target_ip>
```

> ✅ Save the output/screenshot when login is successful.

---

### 2.2 HTTP Login Page

**Tool Used**: Burp Suite (Intruder)

#### Steps:
1. Capture login request using Burp Proxy.
2. Send to Intruder.
3. Set payload positions on username and password.
4. Load username/password list.
5. Start attack and analyze response length/status.

> ✅ Mark the status/length difference for successful login.

---

## 3. Sniff Network Traffic

**Tools Used**: Wireshark or tcpdump

### Steps:
1. Log in using recovered credentials.
2. Start Wireshark:
```bash
wireshark &
```
3. Filter by protocol:
   - FTP: `ftp`
   - TELNET: `telnet`
   - SSH: `ssh`
   - HTTP: `http`

### Observations:
- FTP: ✅ Username & password seen in plaintext.
- TELNET: ✅ Commands and credentials visible.
- SSH: ❌ Encrypted.
- HTTP: ✅ Form data visible.

> ✅ Take screenshots showing plaintext vs encrypted data.

---

## 4. Issues and Solutions

### Issues Encountered:
- SSH too slow → use smaller wordlist.
- HTTP blocking → added delays.

### Fixes:
- Used `-t` option in Hydra to set thread count.
- Used smaller password lists like `top100.txt`.

---

## 5. Mitigation Strategies

| Protocol | Problem | Mitigation |
|---------|---------|------------|
| FTP     | Plaintext | Use **SFTP** |
| TELNET  | Plaintext | Use **SSH** |
| SSH     | Brute force | Use **Key Authentication** and disable password login |
| HTTP    | Plaintext form | Use **HTTPS** |

---

## 6. Summary

**Tools Used**:
- Hydra
- Burp Suite
- Wireshark
- enum4linux / nmap

**Main Steps**:
1. Discovered usernames.
2. Brute-forced logins.
3. Used credentials to log in.
4. Captured traffic and verified encryption.
5. Proposed secure alternatives.

---

## Deliverables

- ✅ Markdown walkthrough (this file)
- ✅ Screenshots (brute force, Wireshark, login success)
- ✅ GitHub repository with all files

---

## GitHub Submission

1. Create a new repository.
2. Upload this walkthrough and screenshot folder.
3. Push using:
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin <repo_url>
git push -u origin main
```

---

## Demo Prep Checklist

- [x] Ready to demo brute-force attack on FTP/TELNET/SSH/HTTP
- [x] Explain why plaintext protocols are insecure
- [x] Show Wireshark evidence
- [x] Propose secure alternatives (SFTP, HTTPS, etc.)

---

## References
- [Hydra](https://github.com/vanhauser-thc/thc-hydra)
- [Burp Suite](https://portswigger.net/burp)
- [Wireshark](https://www.wireshark.org/)


