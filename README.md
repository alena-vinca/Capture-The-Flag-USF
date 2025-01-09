# 🏴‍☠️ **Capture The Flag - USF**

## 📝 **Objective**
This project details the steps taken to complete a **Capture The Flag (CTF)** challenge at **USF**. The goal was to identify and exploit vulnerabilities in a web application to gain unauthorized access.

---

## 🛠️ **Skills Learned**
- SQL Injection techniques
- Web application vulnerability discovery
- Input field testing and manipulation
- Risk mitigation strategies
- Secure coding practices

---

## 🧰 **Tools Used**
- **Web Browser** (Firefox/Chrome)
- **Burp Suite** (optional for further analysis)
- **SQL Injection Payloads**

---

## 📋 **Steps**

### 1️⃣ **Initial Reconnaissance**
1. The provided target IP and port were:
   ```
   83.136.254.158:30497
   ```
2. Accessed the web service by entering the target URL in the browser:
   ```
   http://83.136.254.158:30497
   ```
3. The web service displayed a **login interface**.

---

### 2️⃣ **Testing Input Fields**
1. In the **Username** field, attempted an **SQL injection payload** to bypass authentication:
   ```
   ' OR '1'='1
   ```
2. Initially, this payload returned an error.
3. Modified the payload by adding **admin** to the username field:
   ```
   admin' OR '1'='1
   ```
4. This payload successfully bypassed authentication and granted **admin access** without requiring a password.

---

### 3️⃣ **Observations**
1. The successful login confirmed that the **login page** was vulnerable to **SQL injection**.
2. The backend query was likely structured as follows:
   ```sql
   SELECT * FROM users WHERE username = 'admin' OR '1'='1' AND password = '<password>';
   ```
3. The condition `'1'='1'` always evaluates to **true**, effectively bypassing the authentication controls.

---

### 4️⃣ **Vulnerability Discovery**
The **login page** was vulnerable to a common web application flaw:
- **SQL Injection**: This occurs when user input is directly concatenated into SQL queries without proper input sanitization.

The payload used:
```sql
admin' OR '1'='1
```
Exploited the backend query to bypass the login page's security.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/266d3592-3340-45a2-8c51-f04a67e760d6" />

<img width="600" alt="image" src="https://github.com/user-attachments/assets/70ce440f-2f11-436b-9945-c22d419b9c93" />

---

### 5️⃣ **Immediate Impact**
1. Successfully gaining **admin access** confirmed that the web application was vulnerable.
2. This type of vulnerability could lead to:
   - Unauthorized access to sensitive information
   - Full control over the application
   - Potential manipulation of the database

---

### 6️⃣ **Specific Risk Mitigation**
To prevent SQL injection attacks, the following **security measures** should be implemented:

#### ✅ **Sanitize Input**
- Use **prepared statements** or **parameterized queries** to prevent malicious input from being executed as SQL commands.

#### ✅ **Use Input Validation**
- Enforce strict **input validation rules**, disallowing special characters such as:
   - `'`
   - `--`
   - `;`

#### ✅ **Apply the Principle of Least Privilege**
- Ensure the **database user account** used by the web application has **limited permissions**:
   - **No direct table manipulation** (e.g., `DROP`, `UPDATE`).

#### ✅ **Error Handling**
- Disable **verbose error messages** that expose the **database structure** or **query logic**.

#### ✅ **Implement a Web Application Firewall (WAF)**
- Deploy a **WAF** to monitor and block **malicious inputs**, including SQL injection attempts.

#### ✅ **Regular Security Testing**
- Conduct regular **penetration testing** and **code reviews** to identify and remediate vulnerabilities.

---

## 🎯 **Summary**
In this CTF challenge, the login page was found to be vulnerable to **SQL injection**, which allowed for **unauthorized admin access**. The exploitation demonstrated the importance of **input sanitization** and **secure coding practices** to prevent such vulnerabilities. Risk mitigation strategies were outlined to help secure web applications against similar attacks.

