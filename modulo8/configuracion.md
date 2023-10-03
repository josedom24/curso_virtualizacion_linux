# Configuración de contenedores LXC

El fichero `/etc/lxc/default.conf` contiene la configuración general que van a tener los contenedores que creemos. Su contenido es el siguiente:

```bash
lxc.net.0.type = veth
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up

lxc.apparmor.profile = generated
lxc.apparmor.allow_nesting = 1
```

Como vemos se indica a qué red se va a conectar (`lxc.net.`). Una vez creado un contenedor, el contenido de este fichero se copia a su fichero de configuración (al que se añaden otras configuraciones por defecto). Por ejemplo el fichero de configuración del contenedor `contenedor1` lo encontramos en el fichero `/var/lib/lxc/contenedor1/config`. en este caso, su contenido es:

```bash
lxc.net.0.type = veth
lxc.net.0.hwaddr = 00:16:3e:cf:8f:c3
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
lxc.apparmor.profile = generated
lxc.apparmor.allow_nesting = 1
lxc.rootfs.path = dir:/var/lib/lxc/contenedor1/rootfs

# Common configuration
lxc.include = /usr/share/lxc/config/debian.common.conf

# Container specific configuration
lxc.tty.max = 4
lxc.uts.name = contenedor1
lxc.arch = amd64
lxc.pty.max = 1024
```
Vemos que se han copiado los parámetros de la configuración general (`/etc/lxc/default.conf`) y se han añadido nuevos parámetros: número máximo de terminales (`lxc.tty.max`,`lxc.pty.max`), nombre del contenedor (`lxc.uts.name`), arquitectura (`lxc.arch`), ubicación del sistema de fichero (`lxc.rootfs.path`), ...

Puedes ver los distintos parámetros que podemos incluir en la [documentación oficial](https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html). Por ejemplo si queremos que los contenedores se inicien automáticamente al iniciar el host podríamos:

```
lxc.start.auto = 1
```

Recuerda que tenemos dos opciones:

1. Si escribimos el parámetro en la configuración general, en el fichero `/etc/lxc/default.conf`, afectará a los contenedores que se creen nuevos.
2. Si queremos modificar la configuración de un contenedor ya creado, tenemos que incluir el parámetro en su fichero de configuración, por ejemplo para el `contenedor1` en `/var/lib/lxc/contenedor1/config`.

## Obteniendo información de un contenedor

Para obtener información de un contenedor podemos ejecutar:

```bash
$ lxc-info contenedor1
Name:           contenedor1
State:          RUNNING
PID:            12587
IP:             10.0.3.180
Link:           vethuLaHzY
 TX bytes:      1.70 KiB
 RX bytes:      3.80 KiB
 Total bytes:   5.50 KiB
```

Con la opción `-i` sólo nos da  la dirección ip, con la opción `-S` nos da la estadística de información enviada y recibida por la interfaz de red y con la opción `-s` nos da información del estado.


## Limitando los recursos para los contenedores LXC

Por defectos los contenedores LXC pueden usar todos los recursos de CPU, RAM, disco del host. Podemos limitar estos recursos. El componente del núcleo que posibilita limitar los recursos para cada contenedor son los *Grupos de control* [cgroups](https://wiki.archlinux.org/title/Cgroups), en concreto en Debian 11 y Debian 12 se utiliza [cgroupsv2](https://medium.com/nttlabs/cgroup-v2-596d035be4d7).

Lo primero es que tenemos que habilitar la modificación de la memoria en los *Grupos de control* (cgroups). Para ello tenemos que añadir al fichero `/etc/default/grub` la siguiente información: añadimos al parámetro `GRUB_CMDLINE_LINUX_DEFAULT` el valor `cgroup_enable=memory`. Para que tenga este cambio efecto, reiniciamos la máquina.

Vamos a limitar el uso de memoria RAM (512Mb) y de número de procesadores (1 CPU: la CPU 0) (en la máquina donde estoy corriendo los contenedores tenemos 2Gb de RAM y 2 CPUs), para ello en el fichero de configuración del `contenedor1` indicamos los siguientes parámetros:

```bash
lxc.cgroup2.memory.max = 512M
lxc.cgroup2.cpuset.cpus = 0
```

Reiniciamos el contenedor y comprobamos que se ha llevado a efecto el cambio:

```bash
$ lxc-stop contenedor1
$ lxc-start contenedor1

$ lxc-attach contenedor1 -- free -h
               total        used        free      shared  buff/cache   available
Mem:           512Mi       6.0Mi       505Mi       0.0Ki       0.0Ki       505Mi

$ lxc-attach contenedor1 -- cat /proc/cpuinfo 
processor	: 0
...
```

Aparece un sólo procesador.
