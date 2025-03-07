<hr style="border-color:red;"><h1 align="center"><img src="https://github.com/user-attachments/assets/a5b6e0e5-9274-4b5c-8fd4-c1713624185a"></h1>

<hr style="border-color:red;"><h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1><hr style="border-color:red;">

### **- Verifying the connectivity of the discovered device:**

```bash
ping -c 1 10.10.69.210
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/86175868-81cd-4616-94ee-a886f94599ba"></picture><br>TTL = 63 ==> LINUX OS</h3><hr style="border-color:red;">

### **- Scanning of the discovered device to find all open ports to find possible access routes:**

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.69.210 -oG allPorts
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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e3e6e730-d446-4f9d-8c32-e2aaf34c3922"></picture><br>Accessible port 80 http </h3><hr style="border-color:red;">

### **- Analysis of the open ports found on the discovered device to get more information on the services running on those ports:**

```bash
nmap -sCV -p80 10.10.69.210 -oN targeted
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
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/4673fd49-1a3e-41a1-b8af-d255f7eb410a"></picture><br></h3>

```bash
curl 10.10.69.210/robots.txt
```

<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/5d94029e-df39-47fe-919a-55153a8462f4"></picture><br>Nothing relevant</h3><hr style="border-color:red;">

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1><hr style="border-color:red;">

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

### **Scanning ms17-010 whit METASPLOIT for possible vulnerabilities**
```bash
whatweb "10.10.69.210"
```
```
tool is used for website reconnaissance. It identifies technologies used by a website, such as CMS, frameworks, programming languages, web servers, and security mechanisms.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/7b2ac37a-b551-4641-913b-2e84b2cc54c2"></picture><br>From here we could already look for a vulnerability for FUEL CMS but we lack the correct version.</h3>

### **- From the same web we can find very valuable information:**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/bfa830a5-4450-4d09-8049-f05aff859112"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/4af62892-2d1e-44db-8abc-5bfb2d4592cd"></picture><br></h3>

### **- We will be able to log in with the information found.:**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e073e3af-a053-40b6-bac3-64c53843a6d2"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/3a7340c5-89ec-4d2b-8f1f-24732dc5c354"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/7df5fd21-f5f6-44ea-a101-23123828a7dc"></picture><br>Now we have only confirmed the version of the CMS, now we will have to find a way to break the system..</h3>

### **- looking for possible vulnerabilities**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/83050add-a133-4c69-a1c2-a16aa896f308"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/3465a6a2-122a-4263-b340-324c6cb0f93b"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/56e2983b-2040-4bca-b424-5b87d7e388c8"></picture><br>This sploit requires to be used with python3</h3>

```bash
python3 50477.py -u http://10.10.69.210
```

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/15024ded-0f9f-437b-af24-54ddbfbb0f33"></picture><br>We are in</h3>

### **- Let's remember some information about the web**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e36381fe-462b-4ed6-9cba-6610340c2d2c"></picture><br></h3>

```
cat fuel/application/config/database.php 
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/bbe86f48-8313-45b8-b772-a82bb9783dff"></picture><br>We already have root credentials</h3>

### **- looking for the flags:**
```bash
ls /home/www-data
cat /home/www-data/flag.txt
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/a5a8a433-dc0d-4c85-9ddd-6fc57539a415"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/f57686c8-2894-474c-9662-6e33f048d80b"></picture><br>Even though we have the root credentials we do not have access to</h3>

### **- Now we will create a reverse shell that will give us access from the terminal of our attacking machine:**

```bash
https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php
```

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/f1c71928-734c-46a5-a28a-1b53d5fb914e"></picture><br>We are going to go to the web previously mentioned and copy the code changing the selected lines in the image, we will call this file “shell.php”.</h3>



