# Creación y gestión de contenedores LXC

Al crear un contenedor se bajará el sistema de archivos que formará parte de él. La primera vez que bajamos con `debootstrap` una versión de un sistema operativo se descargará un sistema de archivo mínimo del sistema (que llamamos **plantilla**) que servirá para crear todos los contenedores que creemos de la misma versión del sistema.

La creación de un contenedor con la última versión de debian, se realiza con la instrucción (ejecutada como `root`):

```bash
$ lxc-create -n contenedor1 -t debian -- -r bullseye
```

* `-n`: Nombre del contenedor.
* `-t`: Nombre de la plantilla.
* `-r`: Es una opción de la plantilla. Es el nombre de la versión del sistema operativo. Para ver más opciones de una plantilla ejecutamos: `lxc-create -t debian -h`.

Podemos comprobar que se ha creado un contenedor, ejecutando:

```bash
$ lxc-ls
contenedor1 
```

La plantilla que hemos descargado se guarda en `/var/cache/lxc/debian/rootfs-bullseye-amd64/`. Esta copia del sistema de archivo se utilizará cuando creemos otro contenedor con el mismo sistema operativo. El sistema de archivo del `contenedor1` se guarda en `/var/lib/lxc/contenedor1/rootfs/`.

Ahora podemos iniciar la ejecución del contenedor, comprobar que está funcionando y acceder a él:

```bash
$ lxc-start contenedor1
$ lxc-ls -f
NAME        STATE   AUTOSTART GROUPS IPV4       IPV6 UNPRIVILEGED 
contenedor1 RUNNING 0         -      10.0.3.180 -    false        
$ lxc-attach contenedor1
root@contenedor1:~# 
```

Iniciamos el contenedor con `lxc-start`, comprobamos los contenedores que tenemos creados con `lxc-ls` con la opción `-f` nos da más información (vemos que está ejecutándose, que no se ejecuta al inicio, que ha tomado una dirección ip y que es privilegiado). Por último nos hemos conectado al contenedor con `lxc-attach`.

Podemos parar el contenedor con `lxc-stop` y eliminar el contenedor con `lxc-destroy`.

Para visualizar todas las plantillas que podemos descargar, ejecutamos:

```bash
$ ls /usr/share/lxc/templates/
lxc-alpine    lxc-archlinux  lxc-centos  lxc-debian    lxc-fedora	  lxc-gentoo  lxc-oci		lxc-opensuse  lxc-plamo  lxc-sabayon	lxc-sparclinux	lxc-ubuntu	  lxc-voidlinux
lxc-altlinux  lxc-busybox    lxc-cirros  lxc-download  lxc-fedora-legacy  lxc-local   lxc-openmandriva	lxc-oracle    lxc-pld	 lxc-slackware	lxc-sshd	lxc-ubuntu-cloud
```

También puedes obtener la lista de plantillas en la página [Image server for LXC and LXD](https://uk.lxd.images.canonical.com/).

## Ejecución de comandos en un contenedor

Podemos ejecutar un comando en un contenedor que se esté ejecutando de la siguiente manera:

```bash
$ lxc-attach contenedor1 -- ls -al
```

si el contenedor está apagado, lo haríamos de la siguiente forma:

```bash
$ lxc-stop contenedor1
$ lxc-execute contenedor1 -- ls -al
```
