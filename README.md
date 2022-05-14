# curso_kvm

Curso sobre Qemu/KVM + libvirt

## Contenidos

1. Introducción a la virtualización
	* ¿Qué es la virtualización?
	* Tipos de virtualización
	* Introducción a Qemu/KVM
	* Introducción a libvirt

2. Instalación de Qemu/KVM + libvirt
	* Preparación del escenario de instalación
	* Instalación de Qemu/KVM + libvirt
	* Vista general de virt-manager
	* Vista general de gnome boxes
	* Introducción a virsh
	* Almacenamiento y redes disponibles

3. Creación de máquinas virtuales
	* Dispositivos paravirtualizados. virtio 
	* Creación de máquinas virtuales con virt-install
	* Creación de máquinas virtuales con virt-manager
	* Creación de máquinas virtuales con virsh
	* Creación de máquinas virtuales Windows
	* Instalación de Qemu-guest-agent en las máquinas virtuales
	* Acceso a las máquinas virtuales desde el exterior: ssh RDP, virt-viewer,...

4. Gestión del  almacenamiento en Qemu/KVM + libvirt
	* Introducción al almacenamiento en Qemu/KVM + libvirt
	* Trabajo con imágenes de disco: qemu-img
	* Operaciones sobre volúmenes
	* Añadir nuevos discos a una máquina virtual
	* Creación de un pool de almacenamiento de tipo LVM
	* Aprovisionamiento ligero
	* Instantáneas (snapshots)

5. Clonación de maquinas virtuales
	* Clonación con virt-clone
	* Clonación con virt-manager
	* Clonación manual
	* Clonación usando aprovisionamiento ligero

6. Gestión de redes en Qemu/KVM + libvirt
	* Introducción a la configuración de redes en sistemas virtuales
	* Tipos de redes
	* Redes privadas basadas en NAT
	* Redes privadas aisladas (isolated)
	* Redes públicas conectadas a un bridge externo
	* Configuración de red en las máquina virtuales
	* Introducción a Network Filters
	
	
