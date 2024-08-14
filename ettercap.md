
### **1. Understanding the Basics:**
- **MITM Attack:** In a MITM attack, an attacker intercepts communication between two parties without their knowledge. The attacker can then eavesdrop or manipulate the communication.
- **Ettercap:** Ettercap is a network security tool used for man-in-the-middle attacks on LAN.
- **Wireshark:** Wireshark is a network protocol analyzer that allows you to capture and interactively browse the traffic running on a computer network.

### **2. Setting Up Your Environment:**
- **Operating System:** This guide assumes you're using a Linux-based system (e.g., Kali Linux).
- **Network:** Ensure you're connected to a network where you have permission to test.
- **Install Ettercap and Wireshark:**
  - Install Ettercap: `sudo apt-get install ettercap-graphical`
  - Install Wireshark: `sudo apt-get install wireshark`

### **3. Configuring Ettercap:**
1. **Open Ettercap:**
   - Start Ettercap in graphical mode with the command: `sudo ettercap -G`.

2. **Choosing the Interface:**
   - Go to **"Sniff"** -> **"Unified Sniffing"**.
   - Select the network interface you want to use (e.g., `eth0`, `wlan0`).

3. **Scan for Hosts:**
   - Click on **"Hosts"** -> **"Scan for hosts"**.
   - Ettercap will scan the network for active hosts.

4. **View the Hosts:**
   - Click on **"Hosts"** -> **"Hosts list"**.
   - This will show you a list of IP addresses. Here, you’ll see your target device and the gateway (router).

5. **Add Target:**
   - Select the target device (victim) and add it as Target 1.
   - Select the gateway (router) and add it as Target 2.

6. **ARP Poisoning:**
   - Go to **"Mitm"** -> **"Arp Poisoning"**.
   - Check **"Sniff remote connections"** and click **"OK"**.
   - This step will allow Ettercap to intercept and manipulate the traffic between the target and the gateway.

### **4. Capturing and Analyzing Traffic with Wireshark:**
1. **Start Wireshark:**
   - Open Wireshark and select the network interface that Ettercap is using.

2. **Begin Capturing Traffic:**
   - Click the **"Start capturing packets"** button in Wireshark.

3. **Analyze Traffic:**
   - As the target device browses the web or sends data, you’ll see the packets captured in Wireshark.
   - Filter by specific protocols (e.g., HTTP, TCP) or specific IP addresses to narrow down the traffic.
   - Look for sensitive information, such as unencrypted usernames, passwords, and other data.

4. **Decrypting HTTPS Traffic (Advanced, Optional):**
   - If the traffic is encrypted with HTTPS, you might not see meaningful data without additional steps (like SSL stripping, which requires more advanced configurations).

### **5. Stopping the Attack:**
- **Stop Ettercap:**
  - When you’re done, go back to Ettercap and click on **"Stop"** -> **"Stop Mitm attack"**.
  - Then, stop the packet capture in Wireshark.

### **6. Cleanup and Analysis:**
- **Review the Captured Packets:**
  - Go through the captured packets in Wireshark and analyze them for any sensitive data.
- **Save and Export:**
  - If needed, you can save the captured packets for further analysis by exporting them in a `.pcap` file format.

### **Important Ethical Considerations:**
- **Authorization:** Ensure you have explicit permission to perform these actions. Unauthorized MITM attacks are illegal and unethical.
- **Confidentiality:** Never misuse the information you obtain through these techniques.
