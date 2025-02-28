<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/cec544f6-1a36-4916-b1a9-15ff6d9d63a2"></picture></h1>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1>

```bash
arp-scan -I ens33 --localnet --ignoredups
```
```
arp-scan
This is a command-line tool used to scan a network for active devices using ARP (Address Resolution Protocol). It is useful for discovering devices on a local network.

-I ens33
The -I flag specifies the network interface to use for scanning. In this case, ens33 is the network interface (which may vary depending on your system; you can check available interfaces with ip link show or ifconfig).

--localnet
This tells arp-scan to scan the entire local network based on the subnet assigned to ens33. It automatically calculates the IP range from the interface’s configuration.

--ignoredups
This option ensures that duplicate ARP responses from the same device are ignored, which can be useful in noisy networks where duplicate replies might be common.
```
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/659c585c-6a4c-45de-a16c-1d617d9822ec"></picture></h1>

```bash
ping -c 1 192.168.1.208
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/75c4546b-c569-40ee-b151-01523b49f350"></picture></h1>

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.208 -oG allPorts
```
```
-p-
This tells Nmap to scan all 65,535 TCP ports (from 0 to 65535). By default, Nmap only scans the most common 1,000 ports.

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
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/8d82754c-c473-46fb-9caa-9cc6d87d9e59"></picture></h1>

```bash
nmap -sCV -p22,80 192.168.1.208 -oN targeted
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

-p22,80
Specifies that only ports 22 (SSH) and 80 (HTTP) should be scanned.
This makes the scan faster and more targeted instead of scanning all ports.

-oN targeted
Saves the output in normal format to a file named targeted.
You can later review this file for findings.
```
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/29e653c3-db57-4da4-ba2d-5eefff223189"></picture></h1>

#### **Looking for vulnerabilities with nmap**
```bash
sudo nmap -f --script vuln 192.168.1.208
```
```
-f
Enables packet fragmentation to split probe packets into smaller pieces.
This can help evade firewalls or intrusion detection systems (IDS) by making it harder to detect the scan.
Some firewalls drop fragmented packets, so results may vary.

--script vuln
Runs all vulnerability detection scripts from Nmap’s "vuln" category.
These scripts check for known security vulnerabilities, outdated software, and misconfigurations.
```
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/6a8e9a36-5fe7-464b-83a3-d03328864867"></picture></h1>
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/db041a5f-6890-4e32-88ef-a30db9c06af5"></picture></h1>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1>

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/f214f692-ab8d-4fb8-9f2f-77bdc041e3f0"></picture></h1>
<br>
<br>
``CTRL + u``
<br>
<br> 
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/05bfc834-9e2a-439a-ab0f-4c55c638095e"></picture></h1>

### **Fuzzing**
```bash
gobuster dir -u http://192.168.1.208/ -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -x txt,py,php,sh
```
```
gobuster dir
Uses Gobuster in directory brute-forcing mode.
It attempts to find hidden directories and files on the web server.

-u http://192.168.1.208/
Specifies the target URL (192.168.1.208).
The scan will try to discover directories and files within this web application.

-w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
Specifies the wordlist to use for directory brute-forcing.
This file (directory-list-2.3-medium.txt) contains a large list of common web directories.
Comes from SecLists, a well-known collection of security wordlists.

-x txt,py,php,sh
Adds file extensions to the scan.
Gobuster will test each directory name with these extensions:
.txt → Text files
.py → Python scripts
.php → PHP scripts
.sh → Shell scripts
```
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/61fa3866-184b-435a-a290-480aeed127bf"></picture></h1>

#### **we found these hidden directories**


