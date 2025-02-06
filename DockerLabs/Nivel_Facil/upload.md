
![Pasted image 20250205134718](https://github.com/user-attachments/assets/c559485b-954b-4aaa-b2c9-78db12544ae2)

We test the connection with the victim machine
```bash
ping -c 1 172.17.0.2
```
![image](https://github.com/user-attachments/assets/ca42314b-8c8c-4636-8527-5ec06a9783e6)
ttl =64 ==> linux OS
---------------------------------
As the victim machine is within reach, we will send a request for a response with the ports that are open.
```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
```
![image](https://github.com/user-attachments/assets/cb381a7f-8e0f-4e96-83b5-09b4d86167f5)
-oG AllPorts = save the output in the AllPorts file.

