<hr style="border-color:red;"><h1 align="center"><img src="https://github.com/user-attachments/assets/d66eb6b9-764e-4d6e-add5-56da5e498617"></h1>

<hr style="border-color:red;"><h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1><hr style="border-color:red;">

### **- Verifying the connectivity of the discovered device:**

```bash
ping -c 1 10.10.31.212
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/0f174973-6f6a-45fc-9bd3-63f990d7409e"></picture><br>TTL = 63 ==> LINUX OS</h3><hr style="border-color:red;">

### **- Scanning of the discovered device to find all open ports to find possible access routes:**

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.31.212 -oG allPorts
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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/9aef7a69-1e95-4000-aa38-f3a07c7771b8"></picture><br>Accessible port 22,80,139,445 </h3>

### **- Analysis of the open ports found on the discovered device to get more information on the services running on those ports:**

```bash
nmap -sCV -p80 10.10.31.212 -oN targeted
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
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/541c5431-1b1c-4da8-8323-6b6f59fa5f2d"></picture><br>We have very good information that we can work with</h3>

```bash
curl 10.10.31.212/robots.txt
```
![image](https://github.com/user-attachments/assets/ef51a0aa-d592-4c9f-bd40-c249168b465a)

<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/a6be05b3-5d85-4044-8fab-2385d577d143"></picture><br>A couple of hidden directories of the web mounted on port 80</h3><hr style="border-color:red;">

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

### **This is the web site of the victim machine running on port 80**

<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/9617d934-e90a-4027-8a0d-4c5164b7c7da"></picture><br></h3>

### **we analyze the source code:**
```bash
CTRL+U
```
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/a258606f-f54d-4b42-af83-3547d9a26ead"></picture><br>It seems that when we call the IP 10.10.31.212 it calls this domain //blog.thm and that is why the web page does not load well.</h3>

### **Now we will add the domain //blog.thm to our /etc/hosts so that every time we want to access the ip of the vitima machine through port 80 it will send us directly to the assigned domain.**
```bash
nano /etc/hosts
10.10.31.212 blog.thm
```
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/ef51a0aa-d592-4c9f-bd40-c249168b465a"></picture><br></h3>
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/e3bbdb8c-45d6-40c4-97d9-ba648b2ac1ff"></picture><br>Now we can see the website correctly</h3>
<br>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1><hr style="border-color:red;">

### **- - We will apply web Fuzzing to do a scan for possible hidden directories.**
```bash
gobuster dir -u http://10.10.31.212/ -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
```
```bash
gobuster dir
Uses Gobuster in directory brute-forcing mode.
It attempts to find hidden directories and files on the web server.

-u http://192.168.1.117/
Specifies the target URL (192.168.1.208).
The scan will try to discover directories and files within this web application.

-w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
Specifies the wordlist to use for directory brute-forcing.
This file (directory-list-2.3-medium.txt) contains a large list of common web directories.
Comes from SecLists, a well-known collection of security wordlists.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/58707130-fe54-4463-99ef-d8e24604b901"></picture><br>We found some directories</h3>

### **- - We found this hidden directories**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/3d30501a-e9aa-4a4b-ba83-2a979cedac3d"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/cd4ae524-be0e-4f53-8883-f2cafe38ddd0"></picture><br></h3>

### **- Now that we know that a WordPress is running in the background we can apply a scan to list possible users and plugins.**
```bash
wpscan --url http://blog.thm/ --enumerate u,vp
```
```bash
wpscan → Calls the WPScan tool.
--url http://blog.thm/ → Specifies the target URL (in this case, a local or lab-based WordPress site).
--enumerate u,vp → Enumerates specific components of the WordPress site:
u → Enumerates usernames of registered WordPress users (useful for brute-force attacks).
vp → Enumerates vulnerable plugins installed on the site.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/3220a975-577e-4aa6-a638-ad2d2d9c1633"></picture><br>Although we did not find any plugins, we did manage to find users.<br>kwheel<br>bjoel<br>Karen Wheeler<br>Billy Joel</h3>

### **- Now we will put all those users in a file called users.txt and we will brute force to find possible passwords and be able to enter some user panel.**
```bash
wpscan --url http://blog.thm/ --passwords /usr/share/wordlists/rockyou.txt --usernames user.txt
```
```bash
wpscan → Runs WPScan, a WordPress security scanner.
--url http://blog.thm/ → Specifies the target WordPress website.
--passwords /usr/share/wordlists/rockyou.txt → Uses the RockYou password list for brute-force attempts.
--usernames user.txt → Uses a list of usernames from user.txt instead of a single user.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/2eefa686-dba8-4adc-9868-9bd4e800b53c"></picture><br>We have credentials </h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/5de49747-4210-47cd-9b69-08d3ad62685b"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/4d309cb8-012d-4cf0-97bb-a191a078819d"></picture><br>We are in</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/6501c4d3-6069-43be-8dc1-eefed36de046"></picture><br>We can't upload files with .php .phtml extensions</h3>

### **Let's continue to investigate other ports**
<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>SMB PORT'S 139,445</h2>

### **- List the resources shared by SMB:**
```bash
smbmap -H 10.10.31.212
```
```bash
smbmap → Runs SMBMap, a tool used for SMB share enumeration.
-H 10.10.31.212 → Specifies the target host IP to scan for shared folders
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/d0a0083f-49d2-41a0-9e9c-35d1c26bd04d"></picture><br>We can see that there is a user “BillySMB” who has write and read permissions so we can see what he is sharing over the network and maybe we can find valuable information.</h3>

```bash
smbmap -H 10.10.31.212 -r BillySMB
```
```bash
smbmap → Runs SMBMap, a tool for SMB share enumeration.
-H 10.10.31.212 → Specifies the target IP address of the SMB server.
-r BillySMB → Recursively lists all files and directories in the BillySMB share
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/4d1b1d27-28fa-411b-9dbf-67e56594e8da"></picture><br>let's download the files to see if we get any information</h3>

```bash
smbmap -H 10.10.31.212 --download BillySMB/Alice-White-Rabbit.jpg
smbmap -H 10.10.31.212 --download BillySMB/check-this.png
```
```bash
kitty +kitten icat 10.10.18.139-BillySMB_check-this.png
kitty +kitten icat 10.10.18.139-BillySMB_Alice-White-Rabbit.jpg
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/2ee6acde-5a96-49bd-8da4-3e1aa9e38163"></picture><br>nothing relevant</h3>

### **- Now we can analyze the possibility that the version of WordPress used by this website has a vulnerability.**
<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>WORDPRESS 5.0</h2>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/13a8dff9-f003-43ab-b82c-aac5f86a62c6"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/994e2ae1-ba00-4243-b18e-cd161d871c98"></picture><br></h3>

```bash
searchsploit wordpress 5.0
```
```bash
searchsploit → Runs the SearchSploit tool, which searches a local database of known exploits.
wordpress 5.0 → Searches for exploits specifically related to WordPress version 5.0.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/1b6d092b-aa32-4a8a-aae0-a4ec0994fd62"></picture><br>This has the same code EDB-ID 49512 and also uses python for its execution.</h3>

