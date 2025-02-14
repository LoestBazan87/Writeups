<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/d85d1e02-a6f5-4b24-aaa5-caf437e6d01f"></picture></h1>

## **Visualization of our network card**
```bash
ifconfig
```
![image](https://github.com/user-attachments/assets/f33d0347-0390-4371-9536-5b1f2a0558ed)

## **Scanning our network to find connected devices**
```
arp-scan -I eth0 --localnet --ignoredups
```
![image](https://github.com/user-attachments/assets/d80884f7-3ad7-40c9-80c1-777115ab4438)
```bash
ping -c 1192.168.1.220
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.220 -oG allports
nmap -sCV -p22,80 192.168.1.220 -oN targeted
```







