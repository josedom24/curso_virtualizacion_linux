# Redes en LXC

LXC nos ofrece distintos [mecanismos](https://linuxcontainers.org/lxd/docs/master/networks/) para conectar nuestros contenedores a una red. En este curso nos vamos a centrar en las conexiones de tipo **bridge**. Tenemos dos posibilidades:

* Podemos crear manualmente el *bridge* o usar libvirt para su creación (podemos crear distintos tipos de redes con [libvirt](https://wiki.libvirt.org/page/Networking)).
* Podemos usar `lxc-net`, servicio ofrecido por LXC, que nos facilita la creación de un bridge, que por defecto se llama `lxcbr0`, y que nos ofrece una red de tipo NAT con los servicios de DHCP y DNS.

## Redes con lxc-net

Veamos en primer lugar la segunda opción. El servicio `lxc-net`  crea un bridge llamado `lxcbr0` que nos ofrece una red de tipo NAT con los servicios DHCP y DNS. Por defecto nos ofrece una red con direccionamiento `10.0.3.0/24`.

### Conexión de los contenedores a `lxcbr0`

La configuración por defecto posibilita que los contenedores que creemos se conecten a esta red. Lo podemos ver en la configuración general, en el fichero `/etc/lxc/default.conf`:

```bash
lxc.net.0.type = veth
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
...
```

Si hemos creado un contenedor llamado `contenedor1` recibirá esta configuración que podremos encontrar en su fichero de configuración `/var/lib/lxc/contenedor1/config`:

```bash
lxc.net.0.type = veth
lxc.net.0.hwaddr = 00:16:3e:cf:8f:c3
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
...
```

Por lo tanto podemos comprobar que el `contenedor1` está conectado a esa red. Por ejemplo, si mostramos los contenedores que hemos creado, vemos que ha recibido una ip en ese rango:

```bash
$ lxc-ls -f
NAME        STATE   AUTOSTART GROUPS IPV4       IPV6 UNPRIVILEGED 
contenedor1 RUNNING 1         -      10.0.3.180 -    false        
```

Si accedemos al contenedor podemos hacer varias comprobaciones:

```
$ lxc-attach contenedor1
root@contenedor1:~# ip a
...
2: eth0@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    inet 10.0.3.180/24 brd 10.0.3.255 scope global dynamic eth0
...

root@contenedor1:~# ip r
default via 10.0.3.1 dev eth0 
10.0.3.0/24 dev eth0 proto kernel scope link src 10.0.3.180 

root@contenedor1:~# cat /etc/resolv.conf 
nameserver 10.0.3.1

root@contenedor1:~# apt install iputils-ping
...
root@contenedor1:~# ping www.josedomingo.org
PING endor.josedomingo.org (37.187.119.60) 56(84) bytes of data.
64 bytes from ns330309.ip-37-187-119.eu (37.187.119.60): icmp_seq=1 ttl=49 time=28.7 ms
...
```

1. Comprobamos que se ha configurado con la ip `10.0.3.180`.
2. Vemos que la puerta de enlace es la `10.0.3.1` que corresponde a nuestra máquina física.
3. Del mismo modo la máquina física es el servidor DNS.
4. Hemos instalado la herramienta `ping` y comprobamos que tenemos resolución y acceso al exterior.


## Conexión de los contenedores a un bridge existente

Imaginemos que tenemos en nuestro host instalado libvirt para manejar los recursos de KVM/QEMU y hemos creado una red con `virsh` de tipo NAT, que ha creado un bridge llamado `virbr0`, con las siguientes características:

```
$ virsh net-dumpxml default
<network>
  <name>default</name>
  <uuid>c411a5a1-f998-42a9-bc8a-9a9052fc36f6</uuid>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:fc:32:a2'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.254'/>
    </dhcp>
  </ip>
</network>
```

Podemos modificar el fichero de configuración por defecto `/etc/lxc/default.conf`, indicando el bridge `virbr0`:

```bash
lxc.net.0.type = veth
lxc.net.0.link = virbr0
lxc.net.0.flags = up
...
```

Todos los nuevos contenedores que creemos se conectarán a la red `default`:

```bash
$ lxc-create -n contenedor2 -t debian -- -r bullseye
$ lxc-start contenedor2
$ lxc-ls -f
NAME        STATE   AUTOSTART GROUPS IPV4            IPV6 UNPRIVILEGED 
contenedor1 RUNNING 1         -      10.0.3.10       -    false        
contenedor2 RUNNING 1         -      192.168.122.228 -    false        
```

Vemos como el `contenedor2` ha tomado en una ip de la red `default`.

Si quisiéramos cambiar la conexión del un contenedor ya existente deberíamos hacer la modificación en su fichero de configuración: `/var/lib/lxc/<NOMBRE_CONTENEDOR>/config` y reiniciar el contenedor.

También podríamos conectar el `contenedor1` a la red `default`, para ello vamos a añadir la información de la conexión en su fichero de configuración `/var/lib/lxc/contenedor1/config`:

```
lxc.net.0.type = veth
lxc.net.0.hwaddr = 00:16:3e:cf:8f:c3
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up

lxc.net.1.type = veth
lxc.net.1.hwaddr = 00:16:3e:cf:8f:d3
lxc.net.1.link = virbr0
lxc.net.1.flags = up
...
```

Indicamos la segunda conexión utilizando el nombre de los parámetros como `lxc.net.1.*`. Además hemos cambiado la mac de la segunda tarjeta de red. Ahora reiniciamos y accedemos al contenedor:

```
$ lxc-stop -r contenedor1
$ lxc-attach contenedor1
root@contenedor1:~# apt install nano
root@contenedor1:~# nano /etc/network/interfaces
```

Configuramos la segunda interfaz de red:

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet dhcp
```

Y obtenemos una nueva dirección ip en la nueva red:

```
root@contenedor1:~# ifup eth1
root@contenedor1:~# ip a
...
2: eth0@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
...
    inet 10.0.3.10/24 brd 10.0.3.255 scope global dynamic eth0
...
3: eth1@if14: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
...
    inet 192.168.122.196/24 brd 192.168.122.255 scope global dynamic eth1
...
```

Si listamos los contenedores que tenemos, podemos ver las dos direcciones ip:

```bash
$ lxc-ls -f
NAME        STATE   AUTOSTART GROUPS IPV4                        IPV6 UNPRIVILEGED 
contenedor1 RUNNING 1         -      10.0.3.10, 192.168.122.196  -    false        
contenedor2 RUNNING 1         -      192.168.122.228             -    false    
```
