# Gestión de Redes Puentes (Públicas)

En este apartado vamos  a estudiar como trabajar con las redes puentes.

## Gestión de Redes Puentes conectadas a un bridge externo con virsh

Partimos de que en el host tenemos creado un bridge virtual (que se suele llamar `br0`) al que está conectado el host. Las máquinas virtuales se conectarán en ese bridge y tomarán configuración de red de la misma red a la que está conectada el host. La definición quedaría de la siguiente manera.


La configuración la tenemos en el fichero `red-bridge.xml`, con el contenido:

```xml
<network>
  <name>red_bridge</name>
  <forward mode="bridge"/>
  <bridge name="br0"/>
</network>
```

* El modo de forward se indica como `bridge`.
* Y en la etiqueta `bridge` se pone el nombre del bridge virtual que estamos usando.

Para crear esta nueva red, ejecutamos:

```
virsh -c qemu:///system net-define red-bridge.xml 
La red red_bridge se encuentra definida desde red-bridge.xml
```

La iniciamos:

```
virsh -c qemu:///system net-start red_bridge
La red red_bridge se ha iniciado
```

Y podemos ver que se ha creado:

```
virsh -c qemu:///system net-list --all
 Nombre            Estado     Inicio automático   Persistente
---------------------------------------------------------------
 default           activo     si                  si
 red_aislada       activo     si                  si
 red_bridge        inactivo   no                  si
 red_muy_aislada   inactivo   no                  si
 red_nat           activo     si                  si
 ```

 Desde `virt-manager` no podemos crear una red de este tipo. De todas formas veremos que podemos conectar una máquina virtual directamente al bridge `br0` con lo que será lo mismo que usar la red.

##  Redes Puente compartiendo la interfaz física del host

En este caso vamos a usar una conexión macvtap, que nos permite conectarnos a la red física directamente a través de una interfaz física del host (sin usar un dispositivo bridge). Al igual que con la red anterior, las máquinas virtuales estarán conectados directamente a la red física, por lo que sus direcciones IP estarán todas en la subred de la red física. 
La definición de este tipo de red le hemos guardado en el fichero `red-interface.xml` y sería la siguiente:

```xml
<network>
  <name>red_interface</name>
  <forward mode="bridge">
    <interface dev="enp1s0"/>
  </forward>
</network>
```

Es similar a la anterior, pero se utiliza la etiqueta `<interface>` para indicar el nombre de la interfaz de red física que vamos a utilizar.

Para utilizar este tipo de red, la interfaz que utilicemos no puede estar conectado a un puente virtual.

