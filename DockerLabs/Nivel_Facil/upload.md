<p align="center">
  <img width="460" height="300" src="[https://picsum.photos/460/300](https://github.com/user-attachments/assets/03eb0dba-a1e4-46c6-b617-b6ffe5b806eb)">
![Pasted image 20250205134718](https://github.com/user-attachments/assets/03eb0dba-a1e4-46c6-b617-b6ffe5b806eb)
</p>

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
