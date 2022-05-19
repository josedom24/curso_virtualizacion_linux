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
	* [Conexión local no privilegiada a QEMU/KVM](modulo2/session.md)
	* [Conexión local privilegiada a QEMU/KVM](modulo2/system.md)

3. Creación de máquinas virtuales desde la línea de comandos
	* [Creación de máquinas virtuales con virt-install](modulo3/virt-install.md)
	* [Características de las máquinas virtuales](modulo3/caracteristicas.md)
	* [Gestión de máquinas virtuales con virsh](modulo3/gestion.md)
	* [Definición XML de una máquina virtual](modulo3/xml.md)
	* [Modificación de la definición de una máquina virtual](modulo3/modificacion.md)

4. Creación de máquinas virtuales con virt-manager
	* [Primeros pasos con virt-manager](modulo4/instalacion.md)
	* Vista general de virt-manager
	* Creación de máquinas virtuales (windows) 
	* Gestión de máquinas virtuales
	* Características y hardware de las máquinas virtuales 
	* Instalación de Qemu-guest-agent en las máquinas virtuales
	* Acceso a las máquinas virtuales desde el exterior: ssh RDP, virt-viewer,...

5. Gestión del  almacenamiento en QEMU/KVM + libvirt
	* Introducción al almacenamiento en QEMU/KVM + libvirt
	* Trabajo con imágenes de disco: qemu-img
	* Operaciones sobre volúmenes
	* Añadir nuevos discos a una máquina virtual
	* Creación de un pool de almacenamiento de tipo LVM????
	* Aprovisionamiento ligero
	* Instantáneas (snapshots)

6. Almacenamiento con virt-manager

7. Clonación de maquinas virtuales
	* Clonación con virt-clone
	* Clonación con virt-manager
	* Clonación manual
	* Clonación usando aprovisionamiento ligero
	
8. Gestión de redes en QEMU/KVM + libvirt
	* Introducción a la configuración de redes en sistemas virtuales
	* Tipos de redes
	* Redes privadas basadas en NAT
	* Redes privadas aisladas (isolated)
	* Redes públicas conectadas a un bridge externo
	* Configuración de red en las máquina virtuales
	* Introducción a Network Filters
	
9. Redes con virt-manager	

10. Trabajando con contenedores LXC
