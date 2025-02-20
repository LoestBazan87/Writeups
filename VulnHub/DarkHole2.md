<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/104873e8-46e1-4264-a1e7-51389bc48a1e"></picture></h1>

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Reconnaissance & Scanning</h1>

```bash
arp-scan -I eth0 --localnet --ignoredups
```
![image](https://github.com/user-attachments/assets/de8e5575-0453-497e-80f7-9467daf35c28)

```bash
ping -c 1 192.168.1.210
```
![image](https://github.com/user-attachments/assets/88319795-5f50-45a4-b7e0-4a89fca4c012)

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.210 -oG allPorts
```
![image](https://github.com/user-attachments/assets/caab3713-32f9-45cb-9347-4540c21e16df)

```bash
nmap -sCV -p22,80 192.168.1.210 -oN targeted
```
![image](https://github.com/user-attachments/assets/9d7977fb-b01e-4c8f-9237-8b3dcb959f48)

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Vulnerability Assessment</h1>

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

![image](https://github.com/user-attachments/assets/d42310cb-ab4b-46fe-8bf3-6754bf48e422)

``CTRL+U``
<br>
<br>
![image](https://github.com/user-attachments/assets/94f0d78a-3a61-48f3-b968-07132e49bc7a)
![image](https://github.com/user-attachments/assets/87d49f77-cc5a-4aa5-9daf-eb9186bd72ed)
![image](https://github.com/user-attachments/assets/c8ee583d-de18-4adf-ba69-4fabeb1977e3)<br>
<br>
We found a web login

### **Fuzzing**
```bash
gobuster dir -u http://192.168.1.210/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![image](https://github.com/user-attachments/assets/dbad34e3-bd2f-4f82-922e-97e197aca627)
![image](https://github.com/user-attachments/assets/9821288a-4bc4-40c7-a197-5ee7129a8fde)
![image](https://github.com/user-attachments/assets/8660a7ea-4ec9-4510-87c1-4c3c5c0d98dd)
![image](https://github.com/user-attachments/assets/3216459f-0e6e-4f5b-a488-d0def89b5a41)
![image](https://github.com/user-attachments/assets/2d8a2c67-dfc0-4bde-bfc9-61546d49ea94)
![image](https://github.com/user-attachments/assets/e9754472-778d-4b38-aa5f-35641fe76548)

```bash
whatweb "http://192.168.1.210"
```
![image](https://github.com/user-attachments/assets/9897106f-25dd-4fda-8a3e-f2b6e8d2ef01)








