# Gestión de máquinas virtuales con virsh

[virsh]() es el cliente por línea de comandos "oficial" de libvirt. Ofrece una shell completa para el manejo de la API.

Cuando obtenga la ayuda de esta herramienta verás que en mucha ocasiones habla de dominio. Un **dominio** en QEMU/KVM es una máquina virtual.

Para obtener ayuda sobre todos los comandos que podemos ejecutar:

```
virsh --help
```

Si queremos pedir ayuda de un comando en concreto, por ejemplo el comando `list`, ejecutamos:

```
virsh list --help
```

Ya hemos usado el comando `list` para mostrar las máquinas virtuales que tenemos creada:

```
virsh -c qemu:///system list --all
 Id   Nombre    Estado
----------------------------
 2    prueba1   ejecutando
```

**Nota: Podemos referencia una máquina virtual por su nombre o por su id.**

## Ciclo de vida de una máquina virtual

Para apagar de forma adecuada una máquina virtual:

```
virsh -c qemu:///system shutdown prueba1
Domain 'prueba1' is being shutdown
```

Para iniciar una máquina que está detenida:

```
virsh -c qemu:///system start prueba1
Domain 'prueba1' started
```

Si la propiedad **autostart** de una maquina está activa, cada vez que se inicie el host, esa máquina se encenderá de forma automática. Para activarlo:

```
virsh -c qemu:///system autostart prueba1
Domain 'prueba1' marked as autostarted
```

Reiniciamos una máquina virtual, ejecutando:

```
virsh -c qemu:///system reboot prueba1
Domain 'prueba1' is being rebooted
```

Podemos forzar el apagado de una máquina:

```
virsh -c qemu:///system destroy prueba1
Domain 'prueba1' destroyed
```

Podemos pausar la ejecución de una máquina

```
virsh -c qemu:///system suspend prueba1
Domain 'prueba1' suspended
```

Y continuar la ejecución:

```
virsh -c qemu:///system resume prueba1
Domain 'prueba1' resumed
```

Por último, para eliminar una máquina virtual que esté parada (eliminando los volúmenes asociados):

```
virsh -c qemu:///system undefine --remove-all-storage  prueba1
```

## Obtener información de la máquina virtual

Todos los comandos de `virsh` que empiezan por *dom* nos permiten obtener información de la máquina. 

Para obtener información de la máquina:

```
virsh -c qemu:///system dominfo prueba1 
```

Obtener la dirección IP de la interfaz de red:

```
virsh -c qemu:///system domifaddr prueba1
```

Obtener los discos que tiene la máquina:

```
virsh -c qemu:///system domblklist prueba1
```

Puedes buscar información de más comandos para obtener distinta información de la máquina virtual.



---

[Índice](https://github.com/josedom24/curso_virtualizacion_linux)
