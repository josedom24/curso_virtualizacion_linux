# Configuración de redes virtuales en las máquinas virtuales

Todas las máquinas que hemos creado durante el curso se han conectado de forma predeterminada a la red `default`. 

Sin embargo, en este apartado vamos a aprender algunas cosas nuevas: a crear máquinas virtuales conectadas a otras redes definidas por el usuario y a añadir interfaces de red a máquinas virtuales ya existentes.

## Crear máquinas virtuales conectada a una red existente

Para crear una máquina virtual conectada, por ejemplo, a la red `red_nat`, podemos usar `virt-install`:

```
virt-install --connect qemu:///system \
			 --virt-type kvm \
			 --name prueba5 \
			 --cdrom ~/iso/debian-11.3.0-amd64-netinst.iso \
			 --os-variant debian10 \
			 --network network=red_nat \
			 --disk size=10 \
			 --memory 1024 \
			 --vcpus 1
```

* Con la opción `--network network=red_nat` indicamos que la máquina tendrá una interfaz de red conectada a la red cuyo nombre es `red_nat`.
* Para conectar una máquina a una red también podemos indicar el bridge virtual al que queremos conectarla. en este caso utilizaríamos la opción `--network bridge=virbr1`. `vribr1` es el bridge virtual que gestiona la red `red_nat`.
* Si indicamos varios parámetros `--network`, estaríamos añadiendo a la nueva máquina varias interfaces de red.

Si utilizamos `virt-manager`, para crear la nueva máquina, durante el asistente de creación de la máquina, el el último paso, podemos escoger la red a la que nos vamos a conectar:

![configuración](img/configuracion1.png)

También podemos escoger el puente virtual al que nos queremos conectar:

![configuración](img/configuracion2.png)

## Añadir nuevas interfaces de red a máquinas virtuales

Para añadir una nueva interfaz de red a una máquina virtual, vamos a modificar su definición XML. Podríamos usar `virsh edit` e incluir la definición XML del nuevo disco. Sin embargo, vamos a usar un comando de `virsh` que nos facilita la operación de añadir una nueva interfaz de red y por tanto, la modificación de la definición XML de la máquina.Es recomendable hacer esta operación con la máquina parada.

Por lo tanto, vamos a añadir a la máquina `prueba4` una interfaz de red conectada a la red `red_nat`. Para ello, ejecutamos:

```
virsh -c qemu:///system attach-interface prueba4 network red_nat --model virtio --persistent
La interfaz ha sido asociada exitosamente
```

Si la máquina virtual no tiene entorno gráfico y por tanto no tiene instalado el programa `NetworkManager` habrá que acceder a ella y configurar la nueva interfaz de red.

En nuestro caso es una máquina virtual con Debian 11, donde se ha creado un  interfaz con el nombre `enp10s0`. Para configurarla modificamos el fichero `/etc/network/interfaces`:

![configuración](img/configuracion3.png)

Levantamos la interfaz y comprobamos que la nueva interfaz de red ha tomado configuración de red en el direccionamiento que habíamos configura en la red `red_nat`:

![configuración](img/configuracion4.png)

Además, podríamos ver la configuración de las interfaces de red con el comando `virsh`:

```
virsh -c qemu:///system domifaddr prueba4
 Nombre     dirección MAC       Protocol     Address
-------------------------------------------------------------------------------
 vnet7      52:54:00:d6:0a:19    ipv4         192.168.122.201/24
 vnet8      52:54:00:2a:37:fc    ipv4         192.168.123.225/24
```


Podríamos añadir una nueva interfaz de red indicando el puente virtual al que queremos realizar la conexión. En este caso tendríamos que ejecutar la misma instrucciómn pero el tipo de la conexión será `bridge`:

```
virsh -c qemu:///system attach-interface prueba4 bridge virbr1 --model virtio --persistent
La interfaz ha sido asociada exitosamente
```


Y comprobamos que tenemos una tercera interfaz:

```
virsh -c qemu:///system domiflist prueba4
 Interfaz   Tipo      Fuente    Modelo   MAC
------------------------------------------------------------
 -          network   default   virtio   52:54:00:d6:0a:19
 -          network   red_nat   virtio   52:54:00:2a:37:fc
 -          bridge    virbr1    virtio   52:54:00:0c:06:2a
```

Por último indicar que si queremos desconectar una interfaz de red tenemos que indicar el tipo (`network` o `bridge`) y la dirección MAC:

```
virsh -c qemu:///system detach-interface prueba4 bridge --mac 52:54:00:0c:06:2a --persistent 
La interfaz ha sido desmontada exitosamente
```

También lo podemos hacer desde `virt-manager`. Si **añadimos nuevo hardware** en la vista detalle de la máquina, podemos añadir una nueva conexión indicando la red:

![configuración](img/configuracion5.png)

O indicando el puente virtual donde nos vamos a conectar:

![configuración](img/configuracion6.png)

También podemos modificar en cualquier momento a la red o al puente al que estamos conectado, modificando la interfaz de red desde la vista detalles:

![configuración](img/configuracion7.png)

Para eliminar la interfaz de red desde `virt-manager` simplemente pulsaríamos con el botón derecho sobre el dispositivo de red en la vista detalle, y pulsaríamos sobre **Eliminar Hardware**.


