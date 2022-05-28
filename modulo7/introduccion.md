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

