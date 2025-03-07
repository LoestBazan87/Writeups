<hr style="border-color:red;"><h1 align="center"><img src="https://github.com/user-attachments/assets/f0df1054-6c17-4de7-a559-b483fc2ecf6d"></h1>

<hr style="border-color:red;"><h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1><hr style="border-color:red;">

### **- Verifying the connectivity of the discovered device:**

```bash
ping -c 1 10.10.112.140
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/c6632734-b7e7-494a-b1af-51a091b19054"></picture><br>We are unable to receive the icmp packages back</h3><hr style="border-color:red;">

### **- Scanning of the discovered device to find all open ports to find possible access routes:**

```bash
nmap -p- --open -sSCV --min-rate 5000 -vvv -n -Pn 10.10.112.140 -oG allPorts
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

-sC
Enables Nmap’s default script scanning.
This runs a set of default scripts to gather more information about detected services.
Scripts can detect vulnerabilities, enumerate software versions, and check for misconfigurations.
Equivalent to --script=default.

-sV
Performs version detection of services.
It tries to determine the exact software version running on open ports.

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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/0c7f2d92-7db5-4385-9a95-b37909cc7812"></picture><br>Accessible ports 80 & 3389, We see some information about the “WINDOWS OS” system and it has a web site</h3><hr style="border-color:red;">

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1><hr style="border-color:red;">

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

### **Scanning port 80 for possible vulnerabilities**
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/29ed1447-a54b-4a6d-9bae-800298d35f92"></picture></h3>

```bash
whatweb "10.10.112.140"
```
```
WhatWeb is a fingerprinting tool used to identify web technologies running on a target server or website.
It can detect CMS platforms (WordPress, Joomla, etc.), web frameworks, web server versions, programming languages, and more.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/7cf70d5c-8307-43bb-b16f-b4d372afca27"></picture><br>We obtain some information but not very relevant.</h3>


### **- We analyze the source code for possible information leaks or developer notes:**

```bash
CTRL+U
```

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/9faaed1c-908e-4685-922d-f12812c56d62"></picture><br>We found nothing here</h3>

### **- We will apply web Fuzzing to do a scan for possible hidden directories.**
```bash
gobuster dir -u http://10.10.112.140/ -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
```
```
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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e58aecef-89e7-4a3e-b44f-a8be1d3f63b3"></picture><br>We found some directories</h3>

### **- We found this hidden directories**
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/cdab0a17-197b-425f-8076-ff48d47c5496"></picture><br>As a first point to observe we can see a user “wade”.</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/87a23a02-139d-4b74-b04b-4316ae1aa698"></picture><br>we see that “wade” has commented on “Ready Player One”.</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/637fc9be-d6af-4e3d-a8d6-0e1a71698299"></picture><br>Here we found a note and we don't know what “parzival” is about.</h3>

### **- As we do not know what the note he left himself means and we already have a possible user we could try an RDP connection with these possible credentials and we will use the REMMINA application for this:**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/bf1222a8-8223-473f-8216-43b4103a9f92"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/cf699f36-5ba6-4494-b94c-d449e2a1ca49"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/c9856ad8-3de7-4822-b805-c56bae4ff449"></picture><br>We are in.</h3>

### **- Investigating a bit inside the machine:**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/6b8f1b41-b63d-4d31-8e7b-d7d92d410580"></picture><br>Our first flag</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/d57db402-4aee-48a6-a2f6-2490c55bee8b"></picture><br>We have 3 users but we do not have permissions for the user “Administrator”. </h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/208b496d-c555-4fa5-b302-2d6099bbbbd4"></picture><br>The only thing left for us to investigate is this file</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/97f09c1c-035f-42f9-8614-aeb573db54b7"></picture><br>we do not have a password for the user “administrator”.</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/660eca7d-b346-4f30-931f-13ba93ca65a4"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/f86cb0e6-05de-4efa-a486-6056e27edac0"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/50d1ea42-b770-4d65-9ba1-72d1aa6e5e52"></picture><br>CTRL+S</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/f01878bc-f04c-4717-ba81-20f6dca2419a"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/d199c762-ff4e-4a89-988f-53defcc2ba78"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/ef6280b7-bb9c-4360-9f03-ab459a8be70c"></picture><br>We are nt authority system</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e45a3afd-621b-4794-9d3b-a596694be8c2"></picture><br>We navigate to the Administrator/Desktop directory and find the Root flag</h3>

### **Now we will establish a connection between our console of the attacking machine and the victim machine.:** 
### **- We started METASPLOIT**
```bash
msfdb init && msfconsole
```
```
msfdb init (Initialize Metasploit Database)
msfconsole (Launch Metasploit Console)
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/d43ce66a-11e2-4d5a-8ef2-602e55c310db"></picture><br>Once inside we will use the module 'exploit/multi/script/web_delivery' and configure it to obtain our connection. </h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/727cfc88-4999-4804-90cb-004793a066d7"></picture><br>Now we will enter the required fields</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/b6aee551-0e7f-4382-9bd6-56a75177fc62"></picture></h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/b5619825-8072-4b34-a753-23412e865aa5"></picture><br>we execute with “run -j” and copy the result to the powershell of the windows machine.</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/d420327a-a3b9-4529-af3f-92a99b87c1ce"></picture><br>We execute the command</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/a369b069-7846-4f6e-860e-e5a543e16c8a"></picture><br>we have it and we are inside from the console</h3>
