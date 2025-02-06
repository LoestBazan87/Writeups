
![Pasted image 20250205134718](https://github.com/user-attachments/assets/c559485b-954b-4aaa-b2c9-78db12544ae2)

We test the connection with the victim machine

```bash
ping -c 1 172.17.0.2
```
![image](https://github.com/user-attachments/assets/ca42314b-8c8c-4636-8527-5ec06a9783e6)

    ttl =64 ==> linux OS

As the victim machine is within reach, we will send a request for a response with the ports that are open.

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
```
![image](https://github.com/user-attachments/assets/cb381a7f-8e0f-4e96-83b5-09b4d86167f5)

    -oG AllPorts = save the output in the AllPorts file.

Now we launch a port scan

```bash
nmap -sCV -p80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/be9abb3a-7a49-42e6-94d0-7993b2212504)

With this we only conclude that our attack will focus only on port 80 which is the web page, so we will analyze it.

![image](https://github.com/user-attachments/assets/d60cf894-e4d3-41c4-be9a-1bb7dac9ae1c)

It is a simple website to upload files and store them so we will upload test files to see what formats it accepts.
![image](https://github.com/user-attachments/assets/11ec69e4-ae17-42ea-992f-578f6ac82d21)
![image](https://github.com/user-attachments/assets/6fc481b1-b4d8-40fe-a6c6-b72f6366251f)
![image](https://github.com/user-attachments/assets/bb387ee1-c8a5-4c22-9d4d-53a600cb5005)
![image](https://github.com/user-attachments/assets/fdcaa1af-ce50-4426-82a0-024d6d5ffbdd)

We have managed to upload the files but we do not know where they are so we will apply fuzzing to discover new directories.
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```




