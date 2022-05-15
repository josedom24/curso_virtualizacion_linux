# Introducción a QEMU/KVM

De acuerdo con la [wiki de QEMU](https://wiki.qemu.org/Main_Page), "[QEMU](https://www.qemu.org/) es un emulador genérico y de código abierto de máquinas virtuales.". Se puede usar como emulador, permitiendo ejecutar sistemas operativos de una determinada arquitectura (por ejemplo ARM) sobre otra arquitectura (por ejemplo amd64). Pero también, puede ofrecer una solución de virtualización completa, usando hipervidores como KVM para utilizar las extensiones del procesador para la virtualización y ofrecer un rendimiento mayor.

[KVM](https://www.linux-kvm.org/page/Main_Page), la Maquina virtual basada en el kernel (Kernel-based Virtual Machine), es un hipervisor de tipo 1 integrado al kernel de Linux. Es una solución de virtualización completa para Linux, que contienen las extensiones de virtualización Intel VT o AMD-V. Se compone de un módulo del kernel `kvm.ko`, que provee la infraestructura de virtualización base, y un módulo específico para el tipo de procesador, `kvm-intel.ko` o `kvm-amd.ko`.

Por lo tanto, podemos usar QEMU junto a KVM para permitir la ejecución de máquinas virtuales utilizando imágenes de disco que contienen sistemas operativos sin modificar. Cada máquina virtual tiene su propio hardware virtualizado: una tarjeta de red, discos duros, tarjeta gráfica, ...

## Dispositivos paravirtualizados

Al crear las máquinas virtuales, además de las características básicas como la cantidad de RAM asignada, el espacio de almacenamiento o la CPU, se deben seleccionar los diferentes dispositivos que van a formar parte de ella: interfaz de red, controladores de disco duro, interfaz gráfica, etc. En un sistema de virtualización completa como QEMU/KVM todos los dispositivos están inicialmente emulados por software, de manera que la máquina virtual interactúa con un dispositivo como si lo hiciera con uno físico equivalente. De esta manera podemos encontrar una interfaz de red emulando a la clásica tarjeta de red Realtek 8139 o una interfaz IDE para conectar con un disco duro virtual. Estos dispositivos emulados tienen la ventaja de que pueden utilizar los controladores de dispositivos de sus equivalentes físicos, por lo que se suelen utilizar dispositivos emulados muy comunes, que proporcionan compatibilidad con la mayoría de sistemas operativos y hacen muy sencilla la instalación de los mismos dentro de una máquina virtual. Sin embargo, tienen un inconveniente y es que cuando son dispositivos muy usados, tienen un rendimiento pobre, aumentan el consumo de recursos de la CPU y aumentan la latencia de E/S.

El proyecto KVM proporciona una alternativa al uso de dispositivos emulados, que se conocen como **dispositivos paravirtualizados** y se engloban bajo la denominación [virtIO](https://www.linux-kvm.org/page/Virtio). El nombre de dispositivos paravirtualizados hace referencia a la técnica que utilizan, más cercana a la paravirtualización y que proporciona un rendimiento muy cercano al real, por lo que es muy recomendable utilizar dispositivos virtIO en los dispositivos de E/S que consumen más recursos, por ejemplo, la red y el acceso a discos duros. El único inconveniente que tiene utilizar dispositivos virtIO es que son específicos para KVM y no todos los sistemas operativos los reconocen por defecto. Evidentemente los sistemas linux sí reconocen los dispositivos virtIO y en ese caso siempre es recomendable usarlos, pero otros sistemas operativos, como por ejemplo Windows, no incluyen inicialmente soporte virtio, si queremos usarlos en ese caso, será necesario instalar los controladores de dispositivos durante la instalación del sistema operativo de la máquina virtual.

## libvirt: una API de virtualización

Normalmente no trabajamos directamente con las aplicaciones ofrecidas por QEMU/KVM para la gestión de recursos virtualizados. Es más fácil usar [libvirt](https://libvirt.org/), una API intermedia que nos facilita la comunicación con QEMU/KVM.

---

[Índice](https://github.com/josedom24/curso_virtualizacion_linux)
