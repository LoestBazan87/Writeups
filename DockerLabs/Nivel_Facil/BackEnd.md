![image](https://github.com/user-attachments/assets/ca24b927-fd3b-4463-bd1f-83ff8f8ae5d6)
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/15d9448f-ad52-44d6-b445-0e4cc1708e33)

```bash
whatweb 172.17.0.2
```
![image](https://github.com/user-attachments/assets/0b0201fe-2c6f-4987-8e17-c8c4ff88f0db)

## **We already have some information but in the meantime we will investigate the website**
![image](https://github.com/user-attachments/assets/cbd3cfd0-b9ef-4c4a-b22d-0abac22d0c00)
![image](https://github.com/user-attachments/assets/56104336-5156-430b-8376-de3b4db288b8)

