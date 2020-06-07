# Configurar claves SSH en GitHub

Lo primero es [configurar una clave SSH](crear_claves_ssh.md) en nuestro ordenador.

## Crear una clave SSH en Github
1. Acceder a GitHub
2. Ir a settings
3. Claves SSH
4. Crear nueva clave
    - Poner el nombre a la clave
    - En Key, pegamos el contenido de nuestra clave pública.

Con esto ya hemos configurado nuestra clave pública en GitHub, y ya podemos usarla.

## Usar la clave SSH
En el botón `clonar o descargar`, debemos seleccionar la opción `usar SSH`

- Al clonar un repositorio
```
git clone git@github.com:usuarioGithub/nombreRepositorio.git
``` 
- Al añadir un repositorio remoto a nuestro repositorio local.
```
git remote add origin git@github.com:usuarioGithub/nombreRepositorio.git
``` 
Tras esto debemos subir el contenido de nuestro repositorio a GitHub:
```
git push origin master
```



