
### 1. **Basic Site and File Search**
   - `site:example.com` - Searches only within a specific site.
   - `filetype:pdf` - Finds files with a specific extension, like PDFs.

### 2. **Finding Sensitive Information in Files**
   - `filetype:pdf "confidential"` - Searches PDF files with the word "confidential."
   - `filetype:xls "password"` - Searches Excel files containing the word "password."

### 3. **Searching for Login Pages**
   - `inurl:admin` - Finds URLs with "admin" in them, often revealing login pages.
   - `intitle:"Login" "admin"` - Finds pages with "Login" and "admin" in the title.

### 4. **Finding Exposed Databases**
   - `inurl:phpmyadmin` - Searches for accessible phpMyAdmin login pages.
   - `intitle:"index of" "db_backup"` - Finds pages with database backups.

### 5. **Searching for Password Files**
   - `filetype:log inurl:"password"` - Finds log files that may contain passwords.
   - `filetype:env "DB_PASSWORD"` - Searches for environment files that might expose database credentials.

### 6. **Vulnerable Devices and Open Directories**
   - `inurl:"/wp-content/uploads/"` - Finds open WordPress directories.
   - `intitle:"index of" "backup"` - Finds open directories labeled "backup."

### 7. **Searching for Configuration Files**
   - `intitle:"index of" "config"` - Searches for exposed configuration directories.
   - `filetype:json inurl:config` - Looks for exposed JSON configuration files.

### 8. **Locating Publicly Exposed Cameras**
   - `intitle:"Live View / - AXIS" | inurl:view/view.shtml` - Finds exposed Axis camera feeds.
   - `intitle:"webcamXP 5" inurl:8080` - Searches for public feeds of webcamXP.

### 9. **Looking for Security Vulnerabilities in Web Servers**
   - `intitle:"Apache Status" "Apache Server Status for"` - Finds exposed Apache server status pages.
   - `intitle:"phpinfo()" "PHP Version"` - Searches for PHP info pages that may leak server information.

### 10. **Sensitive Directories and Files**
   - `intitle:"index of" "user"` - Finds directories that might contain user information.
   - `inurl:"/cgi-bin/"` - Searches for CGI directories, which can sometimes contain sensitive scripts.

### 11. **Error Messages that Reveal Information**
   - `intext:"sql syntax near" "unexpected T_STRING"` - Searches for SQL error messages.
   - `intext:"ORA-00933: SQL command not properly ended"` - Finds Oracle SQL error messages.

### 12. **Finding Exposed Emails and Passwords**
   - `intext:"username" filetype:log` - Searches logs for possible usernames.
   - `intext:"password" filetype:txt` - Searches for text files that might expose passwords.

