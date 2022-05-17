# Definición XML de una máquina virtual

Las características, opciones y dispositivos hardware de una máquina virtual están estructuradas con el lenguajes de marcas XML. De la misma forma las características de los distintos recursos con los que podemos trabajar (redes, pools de almacenamiento, volúmenes) también están definidos con XML.

## Esquema XML de una máquina virtual

Para obtener la definición XML de una máquina virtual, ejecutamos la siguiente instrucción:

```
virsh -c qemu:///system  dumpxml prueba1
```

Veamos algunos elementos de la definición:

* El documento XML empieza con la etiqueta `<domain type='kvm' id='6'>` donde se indica el tipo de virtualización utilizada para gestionar la máquina y su identificador.
* El nombre de la máquina se indica con la etiqueta `<name>`.
* La etiqueta `<memory>` nos indica la memoria RAM asignada.
* La vCPU asignadas la encontramos definida en la etiqueta `<vcpu>`.
* Con la etiqueta `<os>` tenemos información de la arquitectura de la máquina virtualizada, además con las etiquetas `<boot>` indicamos el orden de arranque entre distintos dispositivos.
* La información de la CPU la encontramos en la etiqueta `<cpu>`.

Veamos un ejemplo hasta aquí:

```
domain type='kvm' id='6'>
  <name>prueba1</name>
  <uuid>a88eebdc-8a00-4b9d-bf48-cbed7bb448d3</uuid>
  ...
  <memory unit='KiB'>1048576</memory>
  <currentMemory unit='KiB'>1048576</currentMemory>
  <vcpu placement='static'>1</vcpu>
  ...
  <os>
    <type arch='x86_64' machine='pc-q35-5.2'>hvm</type>
    <boot dev='hd'/>
  </os>
  ...
  <cpu mode='custom' match='exact' check='full'>
    <model fallback='forbid'>Cooperlake</model>
    <vendor>Intel</vendor>
    ...
```

A continuación nos encontramos la etiqueta `<devices>` donde se definen los distintos dispositivos hardware que forman parte de la máquina. Veamos algunos ejemplos:

* Los discos se definen con la etiqueta `<disk>`. Encontramos información del tipo (en este caso fichero), tipo del fichero (en este caso qcow2), ruta donde se encuentra el fichero,... Es importante señalar que, por defecto, se configura el disco con un controlador **VirtIO** (`bus='virtio`), es decir, es un dispositivo paravirtualizado que nos ofrece mayor rendimiento. Veamos la definición del disco:

```
  <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source file='/var/lib/libvirt/images/prueba1.qcow2'/>
      <target dev='vda' bus='virtio'/>
      <address type='pci' domain='0x0000' bus='0x04' slot='0x00' function='0x0'/>
    </disk>
```

* Las interfaces de red se definen con la etiqueta `<interface>`. Encontramos información como la mac, la red a la que está conectada (en este caso la red `default`),... También observamos que el modelo de la tarjeta es **VirtIO** (`<model type='virtio'/>`), de nuevo se configura un dispositivo paravirtualizado de alto rendimiento.

```
  <interface type='network'>
      <mac address='52:54:00:8a:50:d1'/>
      <source network='default'/>
      <model type='virtio'/>
      <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
    </interface>
```

* Si nos fijamos en otros dispositivos podremos encontrar la definición del teclado, del ratón, el adaptador gráfico, controladores PCI, CDROM, ...

---

[Índice](https://github.com/josedom24/curso_virtualizacion_linux)
