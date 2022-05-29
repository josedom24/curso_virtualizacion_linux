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

