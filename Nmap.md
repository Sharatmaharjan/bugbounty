### **Nmap (Network Mapper)**

#### **Introduction to Nmap**
Nmap (Network Mapper) is a powerful open-source tool used for network discovery and security auditing. It helps in mapping networks, identifying live hosts, detecting open ports, and determining services and operating systems running on target machines.

#### **Features of Nmap**
- Host discovery
- Port scanning
- Service version detection
- OS detection
- Vulnerability detection
- Scriptable interaction using NSE (Nmap Scripting Engine)

---

### **Basic Nmap Syntax**
```
nmap [options] [target]
```
**Example:**
```
nmap 192.168.1.1
```

---

### **Common Nmap Scans with Explanations and Sample Output**

#### **1. Ping Scan (-sn)**
Used to discover live hosts without port scanning.

**Command:**
```
nmap -sn 192.168.1.0/24
```
**Explanation:**  
- `-sn`: Disables port scanning, only checks for live hosts in the given subnet.

**Sample Output:**
```
Starting Nmap 7.91 ( https://nmap.org ) at 2025-01-26 12:00
Nmap scan report for 192.168.1.1
Host is up (0.0031s latency).
Nmap scan report for 192.168.1.5
Host is up (0.0025s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 2.34 seconds
```

---

#### **2. TCP Connect Scan (-sT)**
Performs a full TCP connection to detect open ports.

**Command:**
```
nmap -sT 192.168.1.1
```
**Explanation:**  
- `-sT`: Conducts a full TCP handshake (useful when SYN scan is not possible).

**Sample Output:**
```
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
443/tcp  closed https
```

---

#### **3. SYN Scan (Stealth Scan) (-sS)**
Most commonly used scan, sends SYN packets without completing a full handshake.

**Command:**
```
nmap -sS 192.168.1.1
```
**Explanation:**  
- `-sS`: Performs a stealthy SYN scan by not completing the handshake.

**Sample Output:**
```
PORT     STATE  SERVICE
21/tcp   closed ftp
22/tcp   open   ssh
80/tcp   open   http
```

---

#### **4. UDP Scan (-sU)**
Scans for open UDP ports.

**Command:**
```
nmap -sU 192.168.1.1
```
**Explanation:**  
- `-sU`: Scans for open UDP services such as DNS (53), SNMP (161), etc.

**Sample Output:**
```
PORT     STATE  SERVICE
53/udp   open   domain
161/udp  open   snmp
```

---

#### **5. Service Version Detection (-sV)**
Identifies services running on open ports.

**Command:**
```
nmap -sV 192.168.1.1
```
**Explanation:**  
- `-sV`: Probes ports to determine the exact service versions.

**Sample Output:**
```
PORT     STATE  SERVICE     VERSION
22/tcp   open   ssh         OpenSSH 8.2p1
80/tcp   open   http        Apache httpd 2.4.41
```

---

#### **6. OS Detection (-O)**
Attempts to identify the operating system of the target.

**Command:**
```
nmap -O 192.168.1.1
```
**Explanation:**  
- `-O`: Uses TCP/IP fingerprinting to guess the operating system.

**Sample Output:**
```
Running: Linux 5.X
OS details: Linux 5.4 - 5.11
```

---

#### **7. Aggressive Scan (-A)**
Combines OS detection, version detection, and traceroute.

**Command:**
```
nmap -A 192.168.1.1
```
**Explanation:**  
- `-A`: Enables aggressive scanning, providing detailed information.

**Sample Output:**
```
PORT     STATE  SERVICE     VERSION
22/tcp   open   ssh         OpenSSH 8.2p1
80/tcp   open   http        Apache httpd 2.4.41
Device type: router
OS details: Linux 5.4 - 5.11
```

---

#### **8. Scan Multiple Targets**
Scan a range of IP addresses or hostnames.

**Command:**
```
nmap 192.168.1.1,192.168.1.2,192.168.1.3
nmap 192.168.1.1-10
```
**Explanation:**  
Scans multiple targets using comma separation or range notation.

---

#### **9. Scan Specific Ports (-p)**
Limits the scan to specified ports.

**Command:**
```
nmap -p 22,80,443 192.168.1.1
```
**Explanation:**  
- `-p`: Specifies ports to scan.

**Sample Output:**
```
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
443/tcp  closed https
```

---

#### **10. Scan All Ports (-p-)**
Scans all 65,535 ports.

**Command:**
```
nmap -p- 192.168.1.1
```
**Explanation:**  
- `-p-`: Scans all TCP ports.

---

#### **11. Nmap Scripting Engine (NSE)**
Performs advanced scanning using built-in scripts.

**Command:**
```
nmap --script=http-title 192.168.1.1
```
**Explanation:**  
- `--script`: Runs an Nmap script, in this case, to fetch the web server title.

**Sample Output:**
```
PORT   STATE SERVICE
80/tcp open  http
| http-title: Welcome to My Website
```

---

#### **12. Saving Scan Results**
Save the scan output in different formats.

**Command:**
```
nmap -oN output.txt 192.168.1.1
nmap -oX output.xml 192.168.1.1
```
**Explanation:**  
- `-oN`: Saves output in normal format.
- `-oX`: Saves output in XML format.

---

### **Practical Usage Scenarios**
1. **Network Inventory:** Find live devices and open services in an organization's network.
2. **Security Auditing:** Identify vulnerable services and misconfigured firewalls.
3. **Penetration Testing:** Check for weak points in systems before exploitation.
4. **Performance Troubleshooting:** Detect open or closed ports that could affect connectivity.

---

### **Best Practices for Using Nmap**
- Always obtain proper authorization before scanning networks.
- Use specific target IPs to avoid unintended scans.
- Combine multiple scanning options for detailed insights.
- Be mindful of firewall policies that may trigger alerts.

---
