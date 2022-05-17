# Modificación de la definición de una máquina virtual

Podemos cambiar la configuración de una máquina virtual modificando su definición XML. Podemos cambiar el nombre, la memoria utilizada, la asignación de CPU, cambiar la configuración de cualquier dispositivo, eliminar o añadir nuevos dispositivos,...

Para realizar la modificación del fichero XML tenemos dos alternativas:

1. Realizar los cambios directamente en el documento XML utilizando el comando `virsh edit`.
2. Utilizando comandos específicos de `virsh` que nos ayudar a realizar el cambio de los distintos parámetros de la configuración.

Hay cambios que se pueden realizar con la máquina funcionando, otros necesitan que la máquina esté parada y otros necesitan un reinicio de la máquina para que se realicen.

Veamos algunos ejemplos:

## Modificar el cambio de una máquina virtual

Este cambio requiere que la máquina esté parada.

```
virsh -c qemu:///system shutdown prueba1
Domain 'prueba1' is being shutdown
```

Tenemos dos opciones para realizar la modificación:

1. Editando directamente el documento XML y cambiando el contenido de la etiqueta `<name>`.

		virsh -c qemu:///system shutdown prueba1
		
