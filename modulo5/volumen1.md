# Gestión de volúmenes de almacenamiento con libvirt

En este apartado vamos a estudiar la gestión de volúmenes de almacenamiento usando la API de libvirt, por lo tanto, utilizando herramientas como `virsh` o `virt-manager`. 

Vamos a trabajar con los Pool de Almacenamiento que hemos creado que son de tipo **dir**, por lo tanto los volúmenes corresponden a ficheros de imágenes de disco. Para estos ejemplos, utilizaremos el formato de imagen **qcow2**.

## Gestión de volúmenes de almacenamiento con virsh

Para obtener los volúmenes de un determinado pool (por ejemplo el pool `default`), ejecutamos:

```
virsh -c qemu:///system vol-list default
 Nombre            Ruta
------------------------------------------------------------
```
**TERMINARLO**

Podemos comprobar que los volúmenes listados se corresponden con ficheros que se encuentran en el directorio del pool `default` (`/var/lib/libvirt/images`).

Podemos obtener la información de un determinado volumen de un pool, ejecutando:

```
virsh -c qemu:///system vol-info vol.qcow2 default
```
**TERMINARLO**

De la misma forma que los pools, los volúmenes están definidos en libvirt con el formato XML. Para ver la definición XML de un volumen del pool `default` podemos ejecutar `virsh -c qemu:///system vol-dumpxml vol.qcow2 default`. A partir de un fichero XML con la definición de un nuevo volumen, podríamos crearlo con el subcomando virsh vol-create. **Nota: En este caso no existe el subcomandos `vol-define`, ya que los volúmenes no se pueden crear temporalmente.**

**Nota: Puedes profundizar en el formato XML que define los volúmenes puedes consultar la documentación oficial: [Storage pool and volume XML format](https://libvirt.org/formatstorage.html).**

Sin embargo, vamos a usar otro comando que nos permite indicar la información del nuevo volumen por medio de parámetros. Vamos a crear un nuevo volumen en el pool `default`, cuyo nombre será `vol1.qcow2`, formato `qcow2` y tamaño de 10G:

```
virsh -c qemu:///system vol-create-as default vol1.qcow2 --format qcow2 10G 
Se ha creado el volumen vol1.qcow2
```

Podemos comprobar que se ha creado un nuevo fichero de imagen:

```
sudo ls -l /var/lib/libvirt/images/
...
-rw------- 1 root         root              196768 may 26 09:24 vol1.qcow2
...
```

También podemos volver a ejecutar `virsh -c qemu:///system vol-list default` para comprobar que se ha creado el volumen.

Para borrar un volumen, ejecutamos:

```
virsh -c qemu:///system vol-delete vol1.qcow2 default
Se ha eliminando el volumen vol1.qcow2
```

Tenemos a nuestra disposición más operaciones sobre los volúmenes, estudiaremos algunas de ellas en apartados posteriores: `vol-clone`, para clonar, `vol-resize`, para redimensionar, `vol-download`, para descargar el volumen en un fichero, `vol-upload`, para cargar información a un volumen desde un fichero,...

**Nota: Hay que recordar que todas estas operaciones se realizan sobre volúmenes, y por tanto el mediod e almacenamiento que gestionan dependerán del tipo del pool con el que estemos trabajando. De esta forma, un `vol-create-as` en un pool de tipo logical crearía un volúmen lógico LVM.**

## Gestión de volúmenes de almacenamiento con virt-manager



