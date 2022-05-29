# Configuración de red en las máquina virtuales

En la creación de las máquinas virtuales podemos elegir la red a la que va a estar conectada la nueva máquina. Durante el curso hemos comprobado que por defecto se conecta a la red `defailt`.

Sin embargo, en este apartado vamos a aprender algunas cosas nuevas: crear nuevas máquinas virtuales conectadas a nuevas redes que hayamos creado y añadir nuevas interfaces de red a las máquinas virtuales.

Además, hay que señalar que a la hora de conectar una máquina virtual a una red lo podremos hacer de dos formas: indicando el nombre de la red, por lo que la máquina se conectará al bridge virtual que está gestionando, o directamente indicando el bridge virtual.

## Selección de la red en la creación de máquinas virtuales

virt-install

virt-manager

Conexión a la nueva red `red-nat`, comprobación de la configuración que se ha obtenido.

Información de red con virsh virt manager

## Añadir nuevas interfaces de red a máquinas virtuales

con virsh

con virt-manager




https://kashyapc.fedorapeople.org/virt/add-network-card-in-guest.txt

attach-interface domain type source [--target target] [--mac mac] [--script script] [--model model] [--config] [--inbound average,peak,burst] [--outbound average,peak,burst]
Attach a new network interface to the domain. type can be either network to indicate a physical network device or bridge to indicate a bridge to a device. source indicates the source device. target allows to indicate the target device in the guest. Names starting with 'vnet' are considered as auto-generated an hence blanked out. mac allows to specify the MAC address of the network interface. script allows to specify a path to a script handling a bridge instead of the default one. model allows to specify the model type. --config indicates the changes will affect the next boot of the domain, for compatibility purposes, --persistent is alias of --config. inbound and outbound control the bandwidth of the interface. peak and burst are optional, so "average,peak", "average,,burst" and "average" are also legal.
Note: the optional target value is the name of a device to be created as the back-end on the node. If not provided a device named "vnetN" or "vifN" will be created automatically.

