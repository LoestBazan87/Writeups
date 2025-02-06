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




