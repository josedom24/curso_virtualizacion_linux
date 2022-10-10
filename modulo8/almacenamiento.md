# Almacenamiento en LXC

Veamos cómo montar un directorio del host en un contenedor. Imaginemos que tenemos el directorio `/opt/contenedor1` con un fichero `index.html` y lo queremos montar en el `contenedor1` en el directorio `/srv/www`. Tenemos que tener en cuenta los siguiente:

El directorio de montaje debe existir en el contenedor:

```
$ lxc-attach contenedor1
root@contenedor1:~# cd /srv
root@contenedor1:/srv# mkdir www
```

En el fichero de configuración del contenedor (`/var/lib/lxc/contenedor1/config`) añadimos la siguiente línea:

```
lxc.mount.entry=/opt/contenedor1 srv/www none bind 0 0
```

Hay que tener en cuenta que al indicar el directorio de montaje hay que usar una ruta relativa (es relativa al directorio donde se encuentra el sistema de fichero del contenedor, en este caso `/var/lib/lxc/contenedor1/rootfs/`).

Reiniciamos el contenedor y comprobamos que se ha montado el directorio de forma correcta:

```
$ lxc-stop contenedor1
$ lxc-start contenedor1
$ lxc-attach contenedor1
root@contenedor1:~# cd /srv/www
root@contenedor1:/srv/www# ls
index.html
```
