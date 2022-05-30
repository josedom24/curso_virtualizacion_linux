# Gestión de Redes Puentes

En este apartado vamos  a estudiar como trabajar con las redes puentes.

## Redes Puente conectadas a un bridge externo

Un bridge externo es un bridge virtual que estarán conectada al router de la red local. El bridge se creará en el servidor donde estamos virtualizando (host). El host estará conectado a este bridge para tener conectividad al exterior. Veamos un esquema:

![bridge externo](img/red_bridge_externo.drawio.png)

* El bridge que vamos a crear lo vamos a llamar `br0`.
* En el host aparecerá una interfaz de red con el mismo nombre que representa la conexión al bridge. Está interfaz de red se configurará de forma estática o dinámica (si la red local tiene un servidor DHCP).
* En el ejemplo vemos que la interfaz física de red es `eth0` que estrá conectada a `br0` para que el host tenga conectividad al exterior. Esa interfaz de red no tendrá asignada dirección IP.
* Posteriormente veremos como podemos conectar las máquinas virtuales a este bridge de tal manera que tomaran direcciones IP en el mismo direccionamiento que el host.

### Creación de un bridge externo con NetworkManager

### Creación de un bridge externo en Debian

### Creación de un bridge externo en Ubuntu

### Gestión de Redes Puentes conectadas a un bridge externo con virsh

### Gestión de Redes Puentes conectadas a un bridge externo con virt-manager

##  Redes Puente compartiendo la interfaz física del host
