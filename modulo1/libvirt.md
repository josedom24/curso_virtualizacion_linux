# Introducción a libvirt

[libvirt](https://libvirt.org/) proporciona una API genérica, un demonio y un conjunto de herramientas de gestión para diferentes sistemas de virtualización, en particular los sistemas de virtualización nativos de linux: KVM, LXC o Xen. También es posible,  manejar a través de libvirt otros sistemas de virtualización como VMware ESXi o Hyper-V.


## Mecanismos de conexión

libvirt proporciona varios mecanismos para conectarse a un hipervisor Qemu/KVM, tanto de forma local como remota, los que veremos en este curso son:


### Acceso local con un usuario no privilegiado

Nos conectamos a la URI `qemu:///session`. Se acceden a las máquinas virtuales de ese usuario. En este modo de conexión, el usuario no suele tener permisos para crear conexiones de red, por lo que se limita su uso de la red no privilegiada de qemu ([SLIRP](https://wiki.qemu.org/Documentation/Networking#User_Networking_.28SLIRP.29)) que es útil para casos simples, pero que tiene bajo rendimiento y es poco configurable. 

### Acceso local privilegiado

Nos conectamos a la URI `qemu:///system`. Se acceden a las máquinas virtuales del sistema. Por las limitaciones vistas anteriormente en el acceso local con usuarios no privilegiados, se utiliza la conexión `qemu:///system`, que es única para todo el sistema y que puede utilizar tanto el usuario `root` como cualquier miembro del grupo `libvirt`.

### Acceso remoto privilegiado por ssh

Nos conectamos a la URI `qemu+ssh:///system`. En las conexiones citadas anteriormente nos conectamos a un socket linux `/var/run/libvirt/libvirt-sock`. A este socket también nos podemos conectar a través de un túnel ssh (qemu+ssh).

## Aplicaciones para usar libvirt

libvirt proporciona una API que puede ser utilizada por diferentes [aplicaciones](https://libvirt.org/apps.html) (CLI, GUI o web). Podemos destacar algunas que vamos a utilizar en este curso:

* **virsh**: Es el cliente por línea de comandos "oficial" de libvirt. Ofrece una shell completa para el manejo de la API.
* **virt-manager**: Es una aplicación gráfica (GUI) que nos proporciona muchas de las funcionalidades para trabajar con libvirt.
* **virtinst**: Paquete que proporciona los comandos `virt-clone`, `virt-install` y `virt-xml` útiles para crear y copiar máquinas virtuales.
* **virt-viewer**: Programa que nos permite acceder a a la consola gráfica de una máquina virtual.
* **gnome-boxes**: Aplicación gráfica muy simple, que utilizando el acceso local con usuario no privilegiado, nos permite gestionar, de forma sencilla, máquinas virtuales.

Cuando cualquier aplicación se conecta a libvirt (con cualquiera de los métodos que hemos estudiado) el formato de la información que se intercambian a través de la API es XML. Puedes encontrar la definición de este formato en la documentación oficial: [XML Format](https://libvirt.org/format.html).

---

[Índice](https://github.com/josedom24/curso_virtualizacion_linux)
