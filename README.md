# Curso virtualización Linux

Curso sobre virtualización en sistemas operativos Linux con QEMU/KVM, libvirt, LXC, ...

## Contenidos

1. Introducción a la virtualización
	* ¿Qué es la virtualización?
	* Tipos de virtualización
	* Introducción a QEMU/KVM
	* Introducción a libvirt

2. Instalación de QEMU/KVM + libvirt
	* Preparación del escenario de instalación
	* Instalación de QEMU/KVM + libvirt	
	* Aplicaciones para la gestión de QEMU/KVM
	* Almacenamiento y redes disponibles

3. Creación de máquinas virtuales desde la línea de comandos
	* Creación de máquinas virtuales con virt-install
	* Creación de máquinas virtuales con virsh
	* Creación de máquinas virtuales con un usuario no privilegiado
	* Creación de máquinas virtuales Windows
	* Instalación de Qemu-guest-agent en las máquinas virtuales
	* Acceso a las máquinas virtuales desde el exterior: ssh RDP, virt-viewer,...

4. Creación de máquinas virtuales con virt-manager
	* Instalación de virt-manager
	* Vista general de virt-manager
	* Creación de máquinas virtuales con virt-manager

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
