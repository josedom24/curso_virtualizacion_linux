# Introducción a LXD

LXD, **Linux Container Daemon**, es una herramienta de gestión de los contenedores y máquinas virtuales del sistema operativo Linux, desarrollada por Canonical.

Internamente usa LXC para la gestión de contenedores, pero facilita el uso de los contenedores añadiendo nuevas funcionalidades.

* LXD no trabaja con plantillas, trabaja con imágenes de sistemas operativos para crear los contenedores. [Lista de imágenes](https://uk.lxd.images.canonical.com/).
* No ofrece soporte para diferentes backends de almacenamiento y tipos de red. Facilitando la gestión de red y almacenamiento.
* LXD ofrece una REST API que podemos usar con una simple herramienta de línea de comandos o con herramientas de terceros.
* LXD gestiona instancias, que pueden ser de tipos: contenedores, usando LXC internamente, y máquinas virtuales usando QEMU internamente.

## Instalación de LXD

Vamos a usar el gestor de paquetes snap (si estás trabajando con la distribución Debian 11 debes instalar el paquete `sanpd`).

```
sudo snap install lxd
```

En la versión Debian 12 lo podemos instalar con `apt`:

```
apt install lxd
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

Hemos dejado todos los valores por defecto, a excepción del tipo de pool de almacenamiento que he indicado que sea un directorio (**dir**). También ha creado una red puente para conectar los contenedores.

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

Para crear una máquina virtual, ejecutamos:

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

## Gestión de imágenes

En la creación de las dos instancias hemos descargado dos imágenes que podemos gestionar con el subcomando `lxc image`, por ejemplo para ver las imágenes que hemos descargado:


```
lxc image  list
+-------+--------------+--------+-------------------------------------+--------------+-----------------+----------+------------------------------+
| ALIAS | FINGERPRINT  | PUBLIC |             DESCRIPTION             | ARCHITECTURE |      TYPE       |   SIZE   |         UPLOAD DATE          |
+-------+--------------+--------+-------------------------------------+--------------+-----------------+----------+------------------------------+
|       | 3aa3fa64e5d0 | no     | Ubuntu focal amd64 (20220911_07:42) | x86_64       | VIRTUAL-MACHINE | 230.98MB | Sep 12, 2022 at 7:28am (UTC) |
+-------+--------------+--------+-------------------------------------+--------------+-----------------+----------+------------------------------+
|       | 7628425e768e | no     | Ubuntu focal amd64 (20220911_07:42) | x86_64       | CONTAINER       | 110.65MB | Sep 12, 2022 at 7:22am (UTC) |
+-------+--------------+--------+-------------------------------------+--------------+-----------------+----------+------------------------------+
```

Para más información ejecuta `lxc image --help`.



## Gestión de instancia

Vamos a usar la utilidad de línea de comandos `lxc`, para ver todas las funcionalidades puedes ejecutar `lxc --help`. Veamos algunas de ella:

* `lxc list`: Listar instancias. Podemos filtrar la lista, por ejemplo `lxc list type=container` o `lxc list ubuntu.*`.
* `lxc info`: Nos da información de una instancia. Con la opción `--show-log` nos muestra los logs de la instancia.
* `lxc start`: Inicia una instancia.
* `lxc stop`: Detiene una instancia.
* `lxc delete`: Borra una instancia.

Si queremos ejecutar un comando en una instancia, ejecutamos:

```
lxc exec <instance_name> -- <command>
```

Por ejemplo:

```
lxc exec ubuntu-container -- apt update
```

Si queremos acceder a una shell de la instancia:

```
lxc exec ubuntu-container -- /bin/bash
```

Si queremos conectarnos a una instancia por una consola, ejecutamos:

```
lxc console <instance_name>
```

Nota: Deberíamos configurar una contraseña para un usuario anteriormente accediendo al bash de la instancia.

## Configuración de las instancias

Tenemos muchos [parámetros de configuración](https://linuxcontainers.org/lxd/docs/master/instances) de las instancias, que podemos devivir en tres bloques: [propiedades de las instancias](https://linuxcontainers.org/lxd/docs/master/instances#properties), [openciones de las instancias](https://linuxcontainers.org/lxd/docs/master/instances#keyvalue-configuration) y [dispositivos de las instancias](https://linuxcontainers.org/lxd/docs/master/instances#device-types).

Al crear una instancia podemos indicar los parámetros de configuración deseados, por ejemplo, si queremos limitar el número de CPU y la memoria de un contenedor, podemos ejecutar:

```
lxc launch images:ubuntu/20.04 ubuntu-limited -c limits.cpu=1 -c limits.memory=192MiB
```

Con el subcomando `lxc config` podemos gestionar la configuración de las instancias, por ejemplo para mostrar la configuración de una instancia, ejecutamos:

```
lxc config show ubuntu-container
```

Por ejemplo, para cambiar un parámetro:

```
lxc config set ubuntu-container limits.memory=128MiB
```

## Para seguir profundizando

* [Más información](https://linuxcontainers.org/lxd/docs/master/configuration/) sobre la configuración de instancias.
* [Más información](https://linuxcontainers.org/lxd/docs/master/images/) sobre la gestión de imágenes.
* Con el subcomando `lxc network` gestionamos las redes. [Más información](https://linuxcontainers.org/lxd/docs/master/networks/).
* Con el subcomando `lxc storage` gestionamos los pools de almacenamiento. [Más información](https://linuxcontainers.org/lxd/docs/master/storage/).
