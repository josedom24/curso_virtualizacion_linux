# Modificación de la definición de una máquina virtual

Podemos cambiar la configuración de una máquina virtual modificando su definición XML. Podemos cambiar el nombre, la memoria utilizada, la asignación de CPU, cambiar la configuración de cualquier dispositivo, eliminar o añadir nuevos dispositivos,...

Para realizar la modificación del fichero XML tenemos dos alternativas:

1. Realizar los cambios directamente en el documento XML utilizando el comando `virsh edit`.
2. Utilizando comandos específicos de `virsh` que nos ayudan a realizar el cambio de los distintos parámetros de la configuración.

Hay cambios que se pueden realizar con la máquina funcionando, otros necesitan que la máquina esté parada y otros necesitan un reinicio de la máquina para que se realicen.

Veamos algunos ejemplos:

## Modificar el nombre de una máquina virtual

En este caso la modificación la vamos a realizar con el comando `virsh domrename`, que modificará internamente la definición XML:

```
virsh -c qemu:///system domrename prueba2 prueba1
Domain 'prueba2' XML configuration edited.
```

Este cambio requiere que la máquina esté parada.

## Modificar la asignación de vCPU

Suponemos que la máquina está parada. Comprobamos el número de vCPU asignadas a la máquina:

```
virsh -c qemu:///system dominfo prueba1
...
CPU(s):         1
...
```

Podemos editar la configuración XML y cambiar el valor de la etiqueta ``<vcpu>`:

```
virsh -c qemu:///system edit prueba1
...
  <vcpu placement='static'>2</vcpu>
...
```

Y volvemos a comprobar la información de la máquina:

```
virsh -c qemu:///system dominfo prueba1
...
CPU(s):         2
...
```

También podríamos cambiar la asignación de vCPU "en caliente" con el camando `virsh setvcpus`, pero no lo vamos a estudiar en este curso. Puedes ver este [árticulo](https://www.unixarena.com/2015/12/linux-kvm-how-to-add-remove-vcpu-to-guest-on-fly.html/) para más información.

## Modificar la asignación de memoria RAM



