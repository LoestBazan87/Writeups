![image](https://github.com/user-attachments/assets/5aeeff3d-98f1-4b30-b5f1-3023544c972f)

We test the connection with the victim machine
```bash
ping -c 1 172.17.0.2
```
![image](https://github.com/user-attachments/assets/daa8b535-5c35-48f1-9480-41ac4c93f05c)

    ttl =64 ==> linux OS

## **As the victim machine is within reach, we will send a request for a response with the ports that are open.**

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
```
![image](https://github.com/user-attachments/assets/5e97d5e8-fbb6-4740-acb3-39d98f2d442b)

    -oG AllPorts = save the output in the AllPorts file.

## **Now we launch a port scan**

```bash
nmap -sCV -p22,80,3000 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/c6ec6892-4d40-44bd-9122-a1378a0182c7)

        -oN targeted = save the output in the targeted file.

