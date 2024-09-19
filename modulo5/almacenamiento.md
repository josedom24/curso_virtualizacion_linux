# Introducción al almacenamiento

El **almacenamiento en disco** es una parte esencial en los sistemas informáticos, ya que permite guardar de forma persistente grandes cantidades de información.

Veamos distintas características del almacenamiento en discos:

## Sistemas de Ficheros vs. Dispositivos de Bloques

* **Dispositivos de Bloques**: 
    * Son unidades (físicas o lógicas) de almacenamiento que gestionan los datos en bloques de tamaño fijo. 
    * Estos dispositivos permiten el acceso directo a cualquier bloque sin necesidad de leer todos los anteriores, lo que facilita la lectura y escritura aleatoria de datos. 
    * Estos dispositivos se pueden particionar, formatear, montar,...
    * Ejemplos: **Discos, particiones, volúmenes lógicos, ficheros llamados imágenes de discos (osi, img, raw, qcow2,...),...**
* **Sistemas de ficheros**:
    * El formateo de un dispositivo de bloque nos permite estructurarlo de forma lógica (ficheros y directorios). Ejemplos: **ext4, xfs, ntfs, btrfs, zfs, ...**
    * Proporcionan una interfaz que permite a los usuarios y aplicaciones almacenar, organizar y acceder a los **archivos y directorios** de manera sencilla.

## Fuente del almacenamiento

* Si el disco está **conectado directamente** en el ordenador se denomina **DAS** (Direct Attached Storage).
* En otras ocasiones el almacenamiento se encuentra en un servidor y se comparte a un cliente. En este caso se denomina **almacenamiento compartido**, y tenemos dos alternativas:
    * * **NAS** (Network Attached Storage): Se comparte por red el almacenamiento en forma de sistema de ficheros. Ejemplos: **nfs, samba, glusterfs,...**
    * **SAN** (Storage Area Network): En una red de almacenamiento se comparte dispositivos de bloques. Ejemplo: **iSCSI, FiberChanel,...**

Dependiendo de la tecnología usada para realizar el **almacenamiento compartido** tendremos varias características:

* **NFS** es un sistema de almacenamiento compartido de tipo NAS, que permite a varios clientes leer y escribir en los ficheros de un mismo directorio. Se pueden tener problemas  de **corrupción de datos** si dos clientes tratan al mismo tiempo de cambiar un mismo fichero.
* **iSCSI** es un sistema de almacenamiento compartido de tipo SAN, que nos permite compartir un dispositivo de bloque entre varios clientes. Si es uno sólo de los clientes el que escribe y los demás leen, no hay ningún problema. Pero si queremos que todos los clientes tengan la posibilidad de leer y escribir, podemos tener problemas de corrupción. En estos casos es necesario usar **sistemas de archivos de clústeres** que añaden mecanismos de bloqueo para que no se pueda cambiar al mismo tiempo un fichero. Por ejemplo: **cfs2, ocfs2, glusterFS, Ceph, ...**

## Snapshots (Instantáneas)

Los snapshots son una característica avanzada de los sistemas de almacenamiento que permiten capturar el estado completo de un sistema de ficheros en un momento dado. Los snapshots son muy útiles para:

* **Recuperación ante fallos**: Si se produce un fallo en el sistema, se puede restaurar el sistema de ficheros al estado en que se encontraba en el momento en que se tomó la instantánea.
* **Pruebas y desarrollo**: Los snapshots permiten hacer pruebas sin riesgo de dañar los datos originales, ya que se puede volver a un estado anterior de forma rápida.

## Aprovisionamiento Ligero (Thin Provisioning)

El aprovisionamiento ligero es una técnica utilizada en sistemas de almacenamiento para optimizar la utilización del espacio disponible. En lugar de asignar todo el espacio de almacenamiento a una unidad o volumen desde el principio (lo que se conoce como **aprovisionamiento grueso o thick provisioning**), el aprovisionamiento ligero asigna espacio según sea necesario.

Por ejemplo, podemos tener un dispositivo de almacenamiento de 30 GB (virtuales), pero que solo ocupa en disco (almacenamiento real) los datos que va guardando. Ejemplos: **Imágenes de disco qcow2, thin-LVM,...**

