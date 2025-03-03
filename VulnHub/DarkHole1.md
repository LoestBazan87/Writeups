<h1 align="center"><picture><img src="https://github.com/user-attachments/assets/d6bb61d9-9d5e-46b4-8e7a-573a4116d536"></picture></h1>

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
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/c8c8d542-3047-44ee-bbb5-00c1f6362d76"></picture></h1>

```bash
ping -c 1 192.168.1.117
```
```bash
ping
The ping command is used to check network connectivity between your machine and another device by sending ICMP (Internet Control Message Protocol) echo request packets.
-c 1
The -c flag specifies the number of echo request packets to send. In this case, -c 1 means only one packet will be sent.
```
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/4772f875-b891-4c88-b43f-5f262c41e038"></picture></h1>

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.117 -oG allPorts
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
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/b9472eda-78e1-4980-b242-40ad7a55f4c3"></picture></h1>

```bash
nmap -sCV -p22,80 192.168.1.117 -oN targeted
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
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/afbee032-4daa-4709-adf0-b93e68107541"></picture></h1>

#### **Looking for vulnerabilities with nmap**
```bash
sudo nmap -f --script vuln 192.168.1.117
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
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/1074dfbc-d980-4883-9657-cbd29ddf8688"></picture></h1>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1>

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

#### **Scanning port 80 for possible vulnerabilities**
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/e03ed69f-7808-4607-819b-0ea465b48384"></picture></h1>

#### **We analyze the source code for possible information leaks or developer notes.**
<br>

`` CTRL+U ``
<br>
<br>
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/d56563a8-061b-47b5-a7af-b6dc0ce1986e"></picture></h1>












