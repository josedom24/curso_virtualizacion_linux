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

1. Editando directamente el documento XML. Tenemos que tener en cuanta que para cada máquina virtual se establece una relación entre su nombre y su UIDD, por lo tanto cambiamos el contenido de la etiqueta `<name>` y eliminamos la etiqueta `<uuid>`.

		virsh -c qemu:///system shutdown prueba1
		
		<domain type='kvm'>
		  <name>prueba2</name>
		  <metadata>
		  ...

		  Domain 'prueba2' XML configuration edited.
		
2. Utilizando el comando `virsh domrename`, que modificará internamente la definición XML:

		virsh -c qemu:///system domrename prueba2 prueba1
		Domain 'prueba2' XML configuration edited.

## Modificar la asignación de vCPU

https://www.unixarena.com/2015/12/linux-kvm-how-to-add-remove-vcpu-to-guest-on-fly.html/

## Modificar la asignación de memoria RAM

https://www.unixarena.com/2015/12/linux-kvm-how-to-add-remove-memory-to-guest-on-fly.html/

