# Clonación con virt-clone

La clonación de una máquina virtual copia la configuración XML de la máquina de origen y sus imágenes de disco, y realiza ajustes en las configuraciones para asegurar la unicidad de la nueva máquina. Esto incluye cambiar el nombre de la máquina y asegurarse de que utiliza los clones de las imágenes de disco. No obstante, los datos almacenados en los discos virtuales del clon son idénticos a los de la máquina de origen. 

La clonación nos permite crear nuevas máquinas de forma muy sencilla, sin necesidad de pasar por el proceso de instalación des de una imagen ISO.

Para realizar la clonación vamos a a partir de una máquina virtual que este apagada.

## Uso virt-clone para realizar la clonación

Vamos a usar la aplicación `virt-clone` para realizar la clonación. Puedes profundizar en el uso de esta herramienta consultando la documentación oficial: [virt-clone](https://linux.die.net/man/1/virt-clone). Veamos algunos casos de uso:

```
virt-clone --connect=qemu:///system --original prueba4 --auto-clone
Asignando 'vol1-clone.qcow2'                               |  10 GB  00:15     

El clon 'prueba4-clone' ha sido creado exitosamente.
```

Es la forma más sencilla, se crea una nueva máquina que se llama como la original y se le añade la palabra `-clone`, todos los discos de la máquina original se clonan, y se llaman igual a los originales con la coletilla `-clone`.

Si queremos indicar el nombre de la nueva máquina:

```
virt-clone --connect=qemu:///system --original prueba4 --name prueba5 --auto-clone
```

