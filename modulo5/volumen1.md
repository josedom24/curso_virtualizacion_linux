# Gestión de volúmenes de almacenamiento con libvirt

En este apartado vamos a estudiar la gestión de volúmenes de almacenamiento usando la API de libvirt, por lo tanto, utilizando herramientas como `virsh` o `virt-manager`. 

Vamos a trabajar con los Pool de Almacenamiento que hemos creado que son de tipo **dir**, por lo tanto los volúmenes corresponden a ficheros de imágenes de disco. Para estos ejemplos, utilizaremos el formato de imagen **qcow2**.

## Gestión de volúmenes de almacenamiento con virsh








virsh vol-create-as prueba vol1.qcow2 --format qcow2 1G 
  525  cd /srv/images/
  526  ls
  527  ls -al
  528  qemu-img info vol1.qcow2 
  529  qemu-img create -f qcow2 vol2.qcow2 2G
  530  virsh vol-list prueba
  531  virsh pool-refresh prueba 
  532  virsh vol-list prueba
  533  virsh vol-info vol1.qcow2 prueba


## Crear una máquina virtual con un volumen ya creado



