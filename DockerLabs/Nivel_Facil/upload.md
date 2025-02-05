<p align="center">
<![Pasted image 20250205134718](https://github.com/user-attachments/assets/03eb0dba-a1e4-46c6-b617-b6ffe5b806eb)>
</p>
![Pasted image 20250205134718](https://github.com/user-attachments/assets/c559485b-954b-4aaa-b2c9-78db12544ae2)

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
