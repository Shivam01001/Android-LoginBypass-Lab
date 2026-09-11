# 🛡️ LoginBypass Android CTF Lab

**LoginBypass** is a custom-built, intentionally vulnerable Android application designed for security researchers, mobile penetration testers, and CTF (Capture The Flag) enthusiasts. 

The application simulates poor coding practices and architectural flaws commonly found in real-world mobile apps, providing a hands-on environment to practice static analysis, dynamic instrumentation, and intent exploitation.

---

## 🏗️ Application Architecture & Flow

The application consists of three primary screens styled with a custom dark terminal aesthetic:
1. **MainActivity:** A classic red-harmless decoy screen containing a system notice warning users away from wasting time.
2. **LoginActivity:** The core authentication gateway featuring custom conditional validation for credentials (`admin` / `Password123`) and specific error handling rules.
3. **DashboardActivity:** The restricted success zone rewarding the hacker with the completion flag message: *"Congratulations Hacker you have did IT, but have you tried any other ways?"*

---

## 🔍 Attack Vectors & Bypass Techniques

As a security lab, this application can be compromised using multiple distinct exploitation vectors. Here are the 5 primary ways to reach the dashboard:

### 1. Static Code Analysis (Hardcoded Credentials)
* **The Vulnerability:** Credentials are hardcoded directly into the Java source code (`LoginActivity.java`).
* **The Exploit:** An attacker can decompile the APK using tools like **JADX** or **Apktool**, navigate through the smali or reconstructed Java code, and extract the cleartext strings (`admin` / `Password123`) instantly without guessing.

### 2. Brute Force & Credential Stuffing
* **The Vulnerability:** Lack of account lockout mechanisms, rate limiting, or CAPTCHA validation on the login form.
* **The Exploit:** An attacker can script an automated script or use an instrumentation framework to rapidly test wordlists against the `LoginActivity` fields until the correct combination is accepted.

### 3. Direct Component Invocation (Activity Bypassing)
* **The Vulnerability:** Misconfigured Android components or exported activities/exported intent permissions.
* **The Exploit:** Using Android Debug Bridge (`adb`), an attacker can bypass the login UI entirely by forcing an explicit intent directly to the restricted destination:
  ```bash
  adb shell am start -n com.example.loginbypass/.DashboardActivity
