<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/2b137a71-3018-4d18-b8cd-04ef8c3918bc"></picture></h1>

## **DISCOVERING**
```bash
arp-scan -I eth0 --localnet --ignoredups
```
![image](https://github.com/user-attachments/assets/330d5006-3db7-4b91-a455-0419280d6267)

```bash
ping -c 1 192.168.1.154
```
![image](https://github.com/user-attachments/assets/b902063a-5b9e-49d3-bb65-272c47d57950)

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.154 -oG allports
```
![image](https://github.com/user-attachments/assets/0043ae4a-6410-468e-9442-8e3eb04d78cd)

```bash
nmap -sCV -p135,139,445,49152,49153,49154,49155,49156,49157 192.168.1.154 -oN targeted
```
![image](https://github.com/user-attachments/assets/96470cbf-bc8c-4c20-84df-f741d31e8875)

## **It is a windows7 machine and has port 445 open so we will test if it is vulnerable to Ethernal Blue.**

