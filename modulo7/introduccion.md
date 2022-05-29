# Introducción a la gestión de redes en libvirt

libvirt nos proporciona las herramientas necesarias para gestionar las redes virtuales a las que se conectan nuestras máquinas virtuales.

Tenemos dos grandes grupos de redes que podemos configurar:

* **Redes Virtuales**: Son **redes privadas** que podemos configurar para que tengan distintas características.
* **Redes Puente**: Las podemos considerar como **redes públicas**, desde el punto de vista que las máquinas virtuales estarán conectadas a la misma red a la que está conectada el host.

Recordemos un **puente o bridge/switch** es un dispositivo de interconexión de redes. La gestión de redes de libvirt se basa en el concepto de **switch virtual**, para ello utiliza **Linux Bridge**, que es un software que nos permite crear bridge virtuales con la misma funcionalidad que un bridge físico.

## Tipos de Redes Virtuales

La clasificación dependerá de la configuración que hagamos a la **Red Virtual**:

### Redes Virtuales de tipo NAT

Es un Red Virtual Privada, las máquinas virtual tendrán un direccionamiento privado y se nos proporciona un mecanismo de **router/nat** para que tengan conectividad al exterior.

![red_nat](img/red_nat.drawio.png)

La red `default` con la que hemos trabajado es de este tipo. Veamos sus características:

* Crea un bridge virtual donde se conectan las máquinas virtuales. En el caso de la red `default` se llama `vmbr0`. A este bridge también está conectado el host.
* Las máquinas virtuales se configuraran de forma dinámica por medio de un servidor DHCP. En el caso de la red `default`, el **rango de direcciones** es `192.168.122.2` - `192.168.122.254`. La **puerta de enlace** de las máquinas se configura con la dirección IP `192.168.122.1` que corresponde al host. El **servidor DHCP** esta configurado en el host. 
* En el host también se configura un **servidor DNS** que es el que se configura en las máquinas virtuales.
* El host hace la función de **router/nat** de tal manera que las máquinas virtuales tienen conectividad al exterior, usando la dirección IP de la interfaz de red del host que está conectada al exterior.

## Redes Virtuales aisladas (Isolated)

Es un Red Virtual Privada, donde las máquinas virtuales tomas direccionamiento privado. No tenemos un mecanismo de router/nat, por lo que las máquinas virtuales no tienen conectividad con el exterior. 

![red_aislada](img/red_aislada.drawio.png)

Por lo tanto tienen las mismas características que una Red Virtual de tipo NAT, pero sin la característica de router/nat. Se gestiona un bridge virtual donde se conectan las máquinas virtuales y el host, seguimos teniendo un servidor DNS y es posible tener un servidor DHCP en el host que asigna dinámicamente un direccionamiento privado a las máquinas.

## Redes Virtuales muy aisladas (Very Isolated)

Es un Red Virtual Aislada, en la que el host no está conectado a las máquians virtuales. Por lo tanto,no tenemos servidor DNS ni DHCP para ser utilizados por las máquinas. Al ser aislada, tampoco tienen salida al exterior.

![red_muy_aislada](img/red_muy_aislada.drawio.png)

En este tipo de red se suele configurar la red de las máquinas virtuales de forma estática.

