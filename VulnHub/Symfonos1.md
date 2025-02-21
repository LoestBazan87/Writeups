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

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

![image](https://github.com/user-attachments/assets/a17e1705-6473-414e-84c7-0a0c9614be6c)<br>
```CTRL + U```<br>
<br>
![image](https://github.com/user-attachments/assets/eee9fbf0-4fdb-4738-b9fb-8561882bc05d)

```bash
whatweb "http://192.168.1.206"
```
![image](https://github.com/user-attachments/assets/d3d9975f-ef95-4a56-ac5a-5b19837293d8)

#### **let's analyze the image we found above**
![image](https://github.com/user-attachments/assets/337c0441-58c3-4b1b-a9cc-6f584b191796)<br>
![image](https://github.com/user-attachments/assets/12484bcf-49f2-4406-9d34-7745fdc4b601)
```bash
wget http://192.168.1.206/image.jpg
```
![image](https://github.com/user-attachments/assets/a9bfcf71-aeba-4659-ab77-b162891dd820)
```bash
exiftool image.jpg
```
![image](https://github.com/user-attachments/assets/f9aa1464-dcf7-45bc-acd4-d02bd04fe942)

### **Fuzzing**
```bash
gobuster dir -u http://192.168.1.206/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![image](https://github.com/user-attachments/assets/a548b799-c922-4c44-9f0d-2ce19fbcc161)
![image](https://github.com/user-attachments/assets/a9a82b2c-5d83-464a-92e1-181c5fa2a7b3)

#### **We have not found anything that can help us at the moment**












