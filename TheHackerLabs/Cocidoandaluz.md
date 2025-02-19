<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/8309287c-1280-4c6c-aab7-4751ac0c2279"></picture></h1>

## **DISCOVERING**

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

## **PORT 80**
![image](https://github.com/user-attachments/assets/f5a410d8-838e-4e26-aaf3-5adee82a12f6)

``CRTL+U``
<br>
<br>
![image](https://github.com/user-attachments/assets/1ad23d93-a26a-4787-b8b8-d1a4b15e0d24)

### **Fuzzing**
```bash
gobuster dir -u http://192.168.1.180/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```




rpcclient -U "" -N 172.17.0.2
srvinfo
querydispinfo
enumdomusers
getdompwinfo





