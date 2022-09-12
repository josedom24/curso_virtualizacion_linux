# Introducción a LXD

LXD, **Linux Container Daemon**, es una herramienta de gestión de los contenedores y máquinas virtuales del sistema operativo Linux, desarrollada por Canonical.

Internamente usa LXC para la gestión de contenedores, pero facilita el uso de los contenedores añadiendo nuevas funcionalidades.

* LXD no trabaja con plantillas, trabaja con imágenes de sistemas operativos para crear los contenedores. [Lista de imágenes](https://uk.lxd.images.canonical.com/).
* No ofrece soporte para diferentes backends de almacenamiento y tipos de red. Facilitando la gestión de red y almacenamiento.
* LXD ofrece una REST API que podemos usar con una simple herramienta de línea de comandos o con herramientas de terceros.
* LXD gestiona instancias, que pueden ser de tipos: contenedores, usando LXC internamente, y máquinas virtuales usando QEMU internamente.

## Instalación de LXD

Vamos a usar el gestor de paquetes snap (si estás trabajando con la distribución Debian debes instalar el paquete `sanpd`).

```
sudo snap install lxd
```

Antes de ejecutar una instancia (contenedor o máquina virtual) tenemos que hacer una configuración inicial de LXD, para ello ejecutamos como root:

```
lxd init
```

Interactivamente nos va preguntando distintos [parámetros](https://linuxcontainers.org/lxd/getting-started-cli/#interactive-setup-options) que nos permitirán realizar la configuración inicial:

```
Would you like to use LXD clustering? (yes/no) [default=no]: 
Do you want to configure a new storage pool? (yes/no) [default=yes]: 
Name of the new storage pool [default=default]: 
Name of the storage backend to use (cephobject, dir, lvm, btrfs, ceph) [default=btrfs]: dir
Would you like to connect to a MAAS server? (yes/no) [default=no]: 
Would you like to create a new local network bridge? (yes/no) [default=yes]: 
What should the new bridge be called? [default=lxdbr0]: 
What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
Would you like the LXD server to be available over the network? (yes/no) [default=no]: 
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]: 
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]: 
```

Hemos dejado todos los valores por defecto, a excepción del tipo de pool de almacenamiento que he indicado que sea un directorio (**dir**). También ha creado un red puente para conectar los contenedores.

## Creación de un contenedor

Para crear un contenedor, ejecutamos:

```
lxc launch <image_server>:<image_name> <instance_name>
```

Por ejemplo:

```
lxc launch images:ubuntu/20.04 ubuntu-container
```

## Creación de máquinas virtuales

Para crear una maquina virtual, ejecutamos:

```
lxc launch <image_server>:<image_name> <instance_name> --vm
```

Por ejemplo:

```
lxc launch images:ubuntu/20.04 ubuntu-vm --vm
```

Y comprobamos que hemos creado un contenedor y una máquina virtual:

```
lxc list
+------------------+---------+-------------------------+-------------------------------------------------+-----------------+-----------+
|       NAME       |  STATE  |          IPV4           |                      IPV6                       |      TYPE       | SNAPSHOTS |
+------------------+---------+-------------------------+-------------------------------------------------+-----------------+-----------+
| ubuntu-container | RUNNING | 10.242.154.69 (eth0)    | fd42:40c4:7788:440e:216:3eff:fe61:66b0 (eth0)   | CONTAINER       | 0         |
+------------------+---------+-------------------------+-------------------------------------------------+-----------------+-----------+
| ubuntu-vm        | RUNNING | 10.242.154.119 (enp5s0) | fd42:40c4:7788:440e:216:3eff:fea3:8748 (enp5s0) | VIRTUAL-MACHINE | 0         |
+------------------+---------+-------------------------+-------------------------------------------------+-----------------+-----------+
```

## Gestión de instancia

Vamos a usar la utilidad de línea de comandos `lxc`, para ver todas las funcionalidades puedes ejecutar `lxc --help`. Veamos algunas de ella:

* `lxc list`: Listar instancias. Podemos filtrar la lista, por ejemplo `lxc list type=container` o `lxc list ubuntu.*`.
* `lxc info`: Nos da información de una instancia. Con la opción `--show-log` nos mietra los logs de la instancia.
* `lxc start`: Inicia una instancia.
* `lxc stop`: Detiene una instancia.
* `lxc delete`: Borra una instancia.



