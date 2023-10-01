# Clonación de máquinas virtuales

La clonación de una máquina virtual copia la configuración XML de la máquina de origen y sus imágenes de disco, y realiza ajustes en las configuraciones para asegurar la unicidad de la nueva máquina. Esto incluye cambiar el nombre de la máquina y asegurarse de que utiliza los clones de las imágenes de disco. No obstante, los datos almacenados en los discos virtuales del clon son idénticos a los de la máquina de origen. 

La clonación nos permite crear nuevas máquinas de forma muy sencilla, sin necesidad de pasar por el proceso de instalación desde una imagen ISO.

Para realizar la clonación vamos a partir de una máquina virtual que esté apagada.

## Uso virt-clone para realizar la clonación

Vamos a usar la aplicación `virt-clone` para realizar la clonación. Puedes profundizar en el uso de esta herramienta consultando la documentación oficial: [virt-clone](https://linux.die.net/man/1/virt-clone). Veamos algunos casos de uso:

```
virt-clone --connect=qemu:///system --original prueba4 --auto-clone
Asignando 'vol1-clone.qcow2'                               |  10 GB  00:15     

El clon 'prueba4-clone' ha sido creado exitosamente.
```

Es la forma más sencilla, se crea una nueva máquina que se llama como la original y se le añade la palabra `--auto-clone`, todos los discos de la máquina original se clonan, y se llaman igual a los originales con la coletilla `--auto-clone`.

Si queremos indicar el nombre de la nueva máquina: usamos el parámetro `--name` y si queremos indicar el nombre del nuevo volumen usamos `--file`:

```
virt-clone --connect=qemu:///system --original prueba4 --name prueba5 --file /var/lib/libvirt/images/vol_prueba5.qcow2 --auto-clone
```
## Uso de virt-manager para realizar la clonación

Si elegimos una máquina virtual y pulsamos el botón derecho del ratón tenemos a nuestra disposición la opción **Clonar**:

![clonación](img/clonacion1.png)

Donde podemos indicar el nombre de la nueva máquina virtual, y si pulsamos sobre el botón **Details...** podemos cambiar el nombre del nuevo fichero de imagen donde se realiza la clonación.

## Las máquinas virtuales clonadas son iguales a las originales

La máquina clon que hemos creado es igual a la original. La nueva máquina contiene identificadores que deberían ser únicos (como el machine ID, direcciones MAC, claves SSH de host, hostname, ...).

![clonación](img/clonacion2.png)

Podemos acceder a la máquina y cambiar el fichero `/etc/hostname` para cambiar el nombre de la máquina, pero todavía tendríamos mucha información repetida entre las dos máquinas. 

Por lo tanto no vamos a realizar la clonación de esta manera. En el siguiente apartado vamos a aprender a crear **plantillas de máquinas virtuales** que nos permiten realizar la clonación de forma adecuada.
