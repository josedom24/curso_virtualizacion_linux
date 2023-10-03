# Definición de Redes Virtuales (Privadas) en libvirt

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
* `<bridge>`: Indicamos el nombre del bridge virtual que se va a utilizar.
* `<forward>`: Indica que las máquinas virtuales van a tener conectividad con el exterior. Por defecto, si no se indicada nada, el tipo es nat: `<forward mode="nat"/>`. El modo también puede ser:
	* `router`: Las redes tipo [router](https://wiki.libvirt.org/page/VirtualNetworking#Routed_mode) también dan acceso a las máquinas virtuales al exterior, pero en ese caso no se utiliza el mecanismo de NAT, sino que se usan rutas de encaminamiento en el host.
	* `open`: Similar a la anterior, excepto que no se añaden reglas de firewall para asegurar que cualquier tráfico pase o no. 
* `<ip>`: Donde se indica la dirección IP y la mascara de red de la dirección del host en la red. Es decir, el host está conectado al bridge virtual con esa dirección.
	* `<dhcp>`: **Este elemento es optativo**. Si queremos tener un servidor DHCP configurado en el host lo configuramos en esta etiqueta, por ejemplo poniendo el rango en la etiqueta `<range>`. 
	
## Definición de Redes Virtuales Aisladas

La definición de una Red Virtual Aislada es igual a la de tipo NAT, pero quitando la etiqueta `<forward>` para deshabilitar la característica de que el host haga router/nat. La definición podría quedar de este modo:

```xml
<network>
  <name>red_aislada</name>
  <bridge name='virbr1'/>
  <ip address='192.168.123.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.123.2' end='192.168.123.254'/>
    </dhcp>
  </ip>
</network>
```

## Definición de Redes Virtuales muy Aisladas
 
Son similares a la anterior, pero el host no se conecta a la red. Por lo tanto no tenemos ni servidor DNS, ni DHCP. Al crear este tipo de red, simplemente se creara un bridge virtual donde se conectarán las máquinas virtuales, que se configurarán de forma estática su direccionamiento. Por lo tanto la definición será:

```xml
<network>
  <name>red_muy_aislada</name>
  <bridge name='virbr2'/>
</network>
```

