<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/88e1fe29-f852-456c-8f61-f7e5fb362ae6"></picture></h1>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1>

```bash
arp-scan -I eth0 --localnet --ignoredups
```
![image](https://github.com/user-attachments/assets/78f2f130-3cff-4270-bb2f-ff47be4a0ec7)

```bash
ping -c 1 192.168.1.206
```
![image](https://github.com/user-attachments/assets/11082534-d4e5-46a4-bd1d-5dcb3739a9cf)

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.206 -oG allPorts
```
![image](https://github.com/user-attachments/assets/aed9999a-d126-4a95-87ca-2191569d542e)

```bash
nmap -sCV -p22,25,80,139,445 192.168.1.206 -oN targeted
```
![image](https://github.com/user-attachments/assets/3fe2fe44-709f-4d84-8e57-2855140ee130)

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1>

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 22</h2>









