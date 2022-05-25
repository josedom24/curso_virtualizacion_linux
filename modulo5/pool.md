# Gestión de Pools de Almacenamiento

## Gestión de Pools de Almacenamiento con virsh

Como hemos visto durante este curso tenemos a nuestra disposición dos Pool de Almacenamiento, para ver los pools con la herramienta `virsh`, ejecutamos la siguiente instrucción:

```
virsh -c qemu:///system pool-list 
 Nombre    Estado   Inicio automático
---------------------------------------
 default   activo   si
 iso       activo   si
```

Recuerda que el pool pode defecto donde se guardan las imágenes de disco es `default`, podemos obtener información de ese pool, con la instrucción:

```
virsh -c qemu:///system pool-info default 
Nombre:         default
UUID:           0a03e05b-8844-4029-8216-430fc289fe8f
Estado:         ejecutando
Persistente:    si
Autoinicio:     si
Capacidad:      87,09 GiB
Ubicación:     36,61 GiB
Disponible:     50,48 GiB
```

Al igual que las máquinas virtuales, los Pools de Almacenamiento se definen por un documento XML. Para ver la definición XML del pool `default` podemos ejecutar `virsh -c qemu:///system pool-dumpxml default`. A partir de un fichero XML con la definición de un nuevo pool, podríamos crearlo con el subcomando `virsh pool-define`. 

Sin embargo, vamos a usar otro comando que nos permite indicar la información del nuevo pool por medio de parámetros. Vamos a crear un nuevo pool que vamos a llamar `mv-images`, de tipo **dir** y cuyo directorio será `/srv/images`. Supongamos que hemos añadido más almacenamiento al host y que hemos montado el disco en el directorio `/srv/images` y queremos guardar las imágenes de disco en esa nueva localización. Para crear el nuevo pool, de forma persistente ejecutamos:

```
virsh -c qemu:///system pool-define-as vm-images dir --target /srv/images
El grupo vm-images ha sido definido
```

A continuación creamos el directorio indicado, con la instrucción:

```
virsh -c qemu:///system pool-build vm-images 
El pool vm-images ha sido compilado
```

Ahora debemos iniciar el pool:

```
virsh -c qemu:///system pool-start vm-images 
Se ha iniciado el grupo vm-images
```

Y si lo deseamos lo podemos auto iniciar, para que en el reinicio del host vuelva a estar activo:

```
virsh -c qemu:///system pool-start vm-images 
Se ha iniciado el grupo vm-images
```

Finalmente vemos la lista de pool y pedimos información del nuevo pool:

```
virsh -c qemu:///system pool-list
 Nombre      Estado   Inicio automático
-----------------------------------------
 default     activo   si
 iso         activo   si
 vm-images   activo   si

virsh -c qemu:///system pool-info vm-images 
Nombre:         vm-images
UUID:           a9eb290a-9973-47ea-b616-0907a5df8ea2
Estado:         ejecutando
Persistente:    si
Autoinicio:     si
...
```


