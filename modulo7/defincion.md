# Definición de redes en libvirt

Las redes que gestiona libvirt se definen con el formato XML. Puedes profundizar en el formato XML con los que se definen las redes consultando el documento [Network XML format](https://libvirt.org/formatnetwork.html). 

## Definición de Redes Virtuales de tipo NAT

La red `default` con la que hemos trabajado es de este tipo. La configuración de la red `default` la podemos encontrar en el fichero `/usr/share/libvirt/networks/default.xml`:

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
	
