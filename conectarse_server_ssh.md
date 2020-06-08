# Para conectarse a un servidor usando SSH

Para poder conectarnos a un servidor mediante SSH, este debe tener instalado SSHD (Servidor SSH - OpenSSH Daemon) y este debe estar ejecutandose.

Para conectarse mediante SSH podemos hacerlo de varias formas:
- Con usuario y contraseña
- Con una clave SSH

## Configurar clave SSH
Para configurar la clave SSH, la primera vez nos deberemos conectar mediante usuario y contraseña, para ello debemos usar el siguiente comando:
```
ssh usuario@ip_server
```
Ejemplo:
```
ssh root@94.27.212.39
```
Si es la primera vez que nos conectamos a ese servidor, nos pedirá confirmación para seguir.  
Con esto se añadirá la IP de ese servidor a listado de servidores conocidos `known_servers`.  
Luego nos pedirá nuestra contraseña y ya estaremos dentro del servidor.

### Añadir nuestra clave en el archivo authorized_keys
En el servidor tenemos que añadir nuestra clave pública en el archivo:  
`~/.ssh/authorized_keys` que contiene las claves públicas de los usuarios que tienen acceso al mismo.  
Para copiar la clave pública en el servidor desde una terminal en nuestro equipo, usamos el siguiente comando:
```
cat ~/.ssh/nombreClave.pub | ssh user@ip_server "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
Nos pedirá el password del usuario y copiará nuestra clave pública dentro del archivo `authorized_keys` del server.

Esto también se puede hacer con dos terminales, una dentro del server, y la otra en el equipo, y copiar el contenido de una en la otra.
Lo primero es comprobar si el usuario tiene una carpeta `.ssh`:
```
cd 
ls -a
```
Si no existe la carpeta, debmos crearla:
```
mkdir ~/.ssh
```
Dentro de la carpeta creamos el archivo `authorized_keys`:
```
cd ~/.ssh
touch authorized_keys
```
Abrimos el archivo con nano, y pegamos el contenido de nuestra clave pública.
```
sudo nano authorized_keys
```

Tras esto ya podemos probar de conectarnos mediante SSH al servidor, para ello usamos el comando:
```
ssh usuario@ip_server
```
Nos pedirá la contraseña de la clave SSH (si tiene), y tras introducirla ya estaremos dentro del servidor.

## Copiar archivos de nuestro ordenador local al servidor
Para crear una copia de un archivo de nuestro pc dentro del servidor al que tenemos acceso por SSH, podemos usar el comando `scp` (Secure Copy):
```
scp ruta/archivo user@ip_server:rutaDestino
```
Ejemplo:
```
scp ~/test.txt root@74.125.69.12:~/archivosSubidos
```


## Crear un nuevo usuario para acceder al servidor
Es una buena práctica, crear un usuario con privilegios sudo, para acceder al servidor y no usar el usuario **root**.  

Para crear un nuevo usuario, usamos el siguiente comando:
```
adduser nombreUsuario
```
A continuación, nos va a pedir el **password**, y rellenar ciertos datos del usuario. (Rellenarlos es opcional).

Para ver la información del nuevo usuario que hemos creado:
```
id nombreUsuario
```
Ahora debemos asignarle a este usuario los privilegios de sudo:
```
usermod -aG sudo nombreUsuario
```
Si volvemos a revisar la información del usuario, veremos que ahora también está incluido en el grupo **sudo**.

Ahora debemos crear la carpeta `.ssh` para este usuario, que es dónde guardaremos sus claves SSH:
```
cd /home/nombreUsuario
mkdir .ssh
cd .ssh
```
Ahora creamos el archivo que contendrá las claves:
```
touch authorized_keys
```
Abrimos otra terminal y copiamos la llave pública del usuario, y la pegamos en el archivo `authorized_keys`:
```
sudo nano authorized_keys
```
Una vez hecho esto, ya nos podemos conectar por SSH con el nuevo usuario.

### Cambiar el propietario de la carpeta .ssh, si la hemos creado con el usuario root
Para cambiar el propietario de la carpeta `.ssh` debemos usar el siguiente comando:
```
sudo chown -R nombreUsuario:nombreUsuario /home/nombreUsuario
```
Con este comando, todo el contenido dentro de la carpeta del usuario, será propiedad del mismo.

### Crear clave SSH para el nuevo usuario
Para poder acceder mediante SSH, cada usuario debe tener su propia clave SSH, por lo que deberemos [crear nueva clave SSH](crear_claves_ssh.md).

Hay que recordar que siempre que creamos una **clave SSH** hay que añadirla al ssh-agent.

## Desactivar el acceso al server usando la contraseña de root

Para esto debemos modificar el archivo de configuración de SSH, para ello usaremos el siguiente comando:
```
sudo nano /etc/ssh/sshd_config
```
Nos pedirá el del usuario con el que estamos logueado en el server.

Dentro de este archivo debemos bajar hasta la línea que dice:
```
PermitRootLogin Yes
```
Y debemos cambiarlo a:
```
PermitRootLogin no
```
Ahora bajamos hasta:
```
PasswordAuthentication no
```
Y comprobar que realmente esté en `no`, si estuviera en `yes`, deberíamos cambiarlo.  

Tras esto, guardamos los cambios `Ctrl + X`, luego `y`, y finalmente `enter`.

Finalmente debemos reiniciar el servicio, para que sean efectivos los cambios:
```
sudo systemctl reload sshd
```
Para cerrar la conexión con el servidor:
```
exit
```
