# Para conectarse a un servidor usando SSH
Para conectarse a un servidor usando ssh, debemos usar el siguiente comando:
```
ssh usuario@ip_server
```
Ejemplo:
```
ssh root@94.27.212.39
```
Si es la primera vez que nos conectamos a ese servidor, nos pedirá confirmación para seguir.  
Con esto se añadirá la IP de ese servidor a listado de servidores conocidos `known_servers`  
Nos pedirá la contraseña de la clave, y tras introducirla ya estaremos dentro del servidor.


## Añadir nuestra clave en el archivo authorized_keys
En el servidor tenemos que añadir nuestra clave pública en el archivo:  
`~/.ssh/authorized_keys`
