# Gestión de Redes Virtuales

En este apartado vamos  a estudiar como trabajar con las redes virtuales con `virsh` y `virt-manager`.

## Gestión de Redes Virtuales con virsh

Podemos ver las redes que tenemos definidas ejecutando:

```
virsh -c qemu:///system net-list --all
```

**TERMINARLO**

Utilizamos la opción `--all` para listar las redes iniciadas y paradas.

Las redes se crean a partir de su definción XML que tenemos guardado en un fichero. En este caso tenemos el fichero `red-nat.xml`, donde tenemos la definción de una red virtual de tipo NAT, con el siguiente contenido:

```xml
<network>
  <name>red_nat</name>
  <bridge name='virbr1'/>
  <forward/>
  <ip address='192.168.123.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.123.2' end='192.168.123.254'/>
    </dhcp>
  </ip>
</network>
```

Para crear la nueva red, ejecutamos:

```
virsh -c qemu:///system net-define red-nat.xml
```

**Terminarlo**

Si utilizamos el comando `virsh create` estaríamos creando la red de forma temporal, no persistente.

La red no se puede utilizar hasta que no se inicie, para ello:

```
virsh -c qemu:///system net-start red_nat
```

**Terminarlo**

Si vamos a usar esta red con muca frecuencia es recomendable activar la propiedad de autoarranque para que se inicie de forma automática al iniciar el host. Para ello:

```
virsh -c qemu:///system net-autostart red_nat
```
**Terminarlo**

Podemos obtener información de la red ejecutando:

```
virsh -c qemu:///system net-info red_nat
```
**Terminarlo**

Al iniciar podemos comprobar que se ha creado el bridge virtual y una nueva interfaz de red en el host.

**Terminarlo**

Podemos crear también una red muy aislada de la que tenemos guardada la definición XML en el fichero `red-muy-aislada.md`, con el contenido:

```xml
<network>
  <name>red_muy_aislada</name>
  <bridge name='virbr2'/>
</network>
```

Y si la creamos y la iniciamos:

```
virsh -c qemu:///system net-define red-nat.xml
virsh -c qemu:///system net-start red-nat.xml
```

**Terminarlo**

Comprobamos que se ha creado el bridge virtual. 

**Terminarlo**




