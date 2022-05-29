# Curso virtualización Linux

Curso sobre virtualización en sistemas operativos Linux con QEMU/KVM, libvirt, LXC, ...

## Contenidos

1. Introducción a la virtualización
	* ¿Qué es la virtualización?
	* Tipos de virtualización
	* [Introducción a QEMU/KVM](modulo1/qemu-kvm.md)
	* [Introducción a libvirt](modulo1/libvirt.md)
	* Introducción a LXC

2. Instalación de QEMU/KVM + libvirt
	* [Preparación del escenario de instalación](modulo2/escenario.md)
	* [Instalación de QEMU/KVM + libvirt](modulo2/instalacion.md)
	* [Conexión local no privilegiada a libvirt](modulo2/session.md)
	* [Conexión local privilegiada a libvirt](modulo2/system.md)
	* Conexión remota a libvirt

3. Creación de máquinas virtuales desde la línea de comandos
	* [Creación de máquinas virtuales con virt-install](modulo3/virt-install.md)
	* [Características de las máquinas virtuales](modulo3/caracteristicas.md)
	* [Gestión de máquinas virtuales con virsh](modulo3/gestion.md)
	* [Definición XML de una máquina virtual](modulo3/xml.md)
	* [Modificación de la definición de una máquina virtual](modulo3/modificacion.md)

4. Creación de máquinas virtuales con virt-manager
	* [Primeros pasos con virt-manager](modulo4/instalacion.md)
	* [Creación de máquinas virtuales Linux](modulo4/linux.md)
	* [Gestión de máquinas virtuales](modulo4/gestion.md)
	* [Detalles de las máquinas virtuales](modulo4/detalles.md)
	* [Creación de máquinas virtuales Windows](modulo4/windows.md)
	* Acceso a las máquinas virtuales desde el exterior

5. Gestión del  almacenamiento en QEMU/KVM + libvirt
	* [Introducción al almacenamiento en QEMU/KVM + libvirt](modulo5/introduccion.md)
	* [Gestión de Pools de Almacenamiento](modulo5/pool.md)
	* [Gestión de volúmenes de almacenamiento con libvirt](modulo5/volumen1.md)
	* [Gestión de volúmenes de almacenamiento con herramientas específicas](modulo5/volumen2.md)
	* [Trabajar con volúmenes en las máquinas virtuales](modulo5/volumen-vm.md)

6. Clonación e instantáneas de maquinas virtuales
	* [Clonación de máquinas virtuales](modulo6/clonacion.md)
	* [Plantillas de máquinas virtuales](modulo6/template.md)	
	* [Clonación completa a partir de plantillas](modulo6/completa.md)
	* [Clonación ligera a partir de plantillas](modulo6/ligera.md)
	* [Instantáneas de máquinas virtuales](modulo6/snapshot.md)
	
7. Gestión de redes en QEMU/KVM + libvirt
	* [Introducción a la gestión de redes en QEMU/KVM + libvirt](modulo7/introduccion.md)
	* [Definición de redes en libvirt](modulo7/definicion.md) 
	* [Gestión de Redes Virtuales](modulo7/virtuales.md)
	* Gestión de Redes Puentes
	* Configuración de red en las máquina virtuales
	
8. Trabajando con contenedores LXC
