# Introducción a la gestión de redes en libvirt

libvirt nos proporciona las herramientas necesarias para gestionar las redes virtuales a las que se conectan nuestras máquinas virtuales.

Tenemos dos grandes grupos de redes que podemos configurar:

* **Redes Virtuales**: Son **redes privadas** que podemos configurar para que tengan distintas características.
* **Redes Puente**: Las podemos considerar como **redes públicas**, desde el punto de vista que las máquinas virtuales estarán conectadas a la misma red a la que está conectada el host.

Recordemos un **puente o bridge/switch** es un dispositivo de interconexión de redes. La gestión de redes de libvirt se basa en el concepto de **switch virtual**, para ello utiliza **Linux Bridge**, que es un software que nos ofrece la misma funcionalidad que un bridge físico.

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

Las redes que gestiona libvirt se definen con el formato XML. Puedes profundizar en el formato XML con los que se definen las redes consultando el documento [Network XML format](https://libvirt.org/formatnetwork.html). La configuración de la red `default` la podemos encontrar en el fichero `/usr/share/libvirt/networks/default.xml`:

```xml
<network>
  <name>default</name>
  <bridge name='virbr0'/>
  <forward/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.254'/>
    </dhcp>
  </ip>
</network>
```

Veamos las etiquetas:

* `<name>`: Nombre de la red.
* `<bridge>`: Indicamos el nombre del bidge virtual que se va a utilizar.
* `<forward>`: Indica que las máquinas virtuales van a tener conectividad con el exterior. Por defecto, si no se indicada nada, el tipo es nat: `<forward mode="nat"/>`. El modo también puede ser `router`: Las redes tipo [router](https://wiki.libvirt.org/page/VirtualNetworking#Routed_mode) también dan acceso a las máquinas virtuales al exterior, pero en ese caso no se utiliza el mecanismo de NAT, sino que se usan rutas de encaminamiento en el host.
* `<ip>`: Donde se indica la dirección IP y la mascara de red de la dirección del host en la red. Es decir, el host está conectado al bridge virtual con esa dirección.
	* `<dhcp>`: **Este elemento es optativo**. Si queremos tener un servidor DHCP configurado en el host lo configuramos en esta etiqueta, por ejemplo poniendo el rango en la etiqueta `<range>`. 
	
