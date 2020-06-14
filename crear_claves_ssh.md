# Claves SSH

Lo primero es comprobar que tenemos un **SSH-Server** instalado en nuestro ordenador.  

Si no lo tenemos podemos instalarlo, según nuestro sistema operativo:

- WINDOWS - La forma más fácil es instalar git, que por defecto nos instala un servidor SSH-Server, en concreto OpenSSH.
  - Realizar todos los comandos desde la consola Git Bash
- Linux - Por defecto viene instalado en la mayoría de distribuciones, pero si no estuviera instalado, lo podemos instalar con el siguiente comando
  - `sudo apt install openssh-server`

Si lo tenemos instalado, tenemos que comprobar que exista el directorio `/home/nombreUsuario/.ssh`.  
Si no existe tenemos que crearlo:

```bash
mkdir ~/.ssh
```

Es necesario que este directorio exista, porque es donde se van a guardar las claves SSH.

También es necesario tener instalado **GIT** y haber hecho las configuraciones mínimas, user.name y user.email  

- Ya que la clave generada tendrá en cuenta estas configuraciones

## Crear clave SSH

Para generar una nueva clave tenemos que ejecutar el siguiente comando:

```bash
ssh-keygen
```

A continuación se nos pide el nombre del archivo en el que guardaremos la clave:

- Pulsando `enter` nos crea el archivo con el nombre por defecto:
  - `/home/nombreUsuario/.ssh/id_rsa`
- Podemos usar cualquier otro nombre para el archivo:
  - `/home/nombreUsuario/.ssh/nombreArchivoClave`

En el siguiente paso se nos solicita una contraseña para esta clave:
> Es recomendable ponerle una contraseña, ya que si alguien consiguiera nuestra clave privada, no podría usarla.  

Ahora hay que confirmar la contraseña, si la hemos puesto en el paso anterior.

Con esto ya tenemos creada la clave SSH, este proceso nos crea dos archivos dentro de la carpeta `.ssh`:

- **id_rsa** - Es la clave privada, no debemos compartirla con nadie.
- **id_rsa.pub** - Este archivo contiene la clave pública, es la que compartiremos para conectarnos de forma segura con SSH.

Podemos examinar el conenido de la clave pública con el comando:

```bash
cat ~/.ssh/id_rsa.pub
```

Desde aquí podremos copiar el contenido del archivo para crear claves SSH en servidores y servicios web.

## Agregar clave al agente SSH-agent

Para comprobar si está activo el `SSH-agent`:

```bash
ssh-agent
```

Si está funcionando debe aparecer un mensaje tipo:

```bash
# Output

SSH_AUTH_SOCK=/tmp/ssh-8csWUH6nqg3q/agent.40619; export SSH_AUTH_SOCK;
SSH_AGENT_PID=40620; export SSH_AGENT_PID;
echo Agent pid 40620;
```

Para iniciar el agente SSH, usamos el siguiente comando:

```bash
eval $(ssh-agent -s)
```

También se puede usar esta sintaxis:

```bash
eval `ssh-agent -s`
```

Nos debe aparecer un mensaje parecido a este:

```bash
Agent pid 20248
```

Para añadir la clave al agente usamos el comando:

```bash
ssh-add ~/.ssh/id_rsa
```

**¡¡Importante!! debemos añadir la clave privada, no la pública.**  

En este punto nos puede pedir la contraseña de la clave, en el caso que le hayamos creado una en el momento de crear la clave.

Con esto ya hemos registrado nuestra clave SSH y ya está lista para ser usada.

### Activar el SSH-Agent en Windows

En la barra de herramientas, en la herramienta de búsqueda, tecleamos `servicios`  

- Ahora pulsamos el botón derecho -> Ejecutatar como administrador.
- En el listado de servicios, buscamos el servicio `Open SSH Authorization Agent`
- Damos doble click sobre este servicio, y seleccionamos habilitarlo, con tipo de inicio automático.
- Finalmente pulsamos en el botón de `Iniciar`, para iniciar el servicio.

### Para añadir la clave creada, desde la terminal Windows

Para añadir la clave desde la terminal de Windows, debemos usar la ruta absoluta:

```terminal
C:\Users\nombreUsuario\.ssh/nombre_clave_rsa
```

Recordar que siempre se añade la clave privada.
Algunas veces da fallos, por lo que recomiendo hacerlo desde la terminal `git bash`
