# Características de las máquinas virtuales

Después de instalar nuestra primera máquina, podemos comprobar la lista de máquinas ejecutando la siguiente instrucción:

```
virsh -c qemu:///system list
 Id   Nombre    Estado
----------------------------
 2    prueba1   ejecutando
```

Si queremos acceder a la terminal de una máquina podemos usar `virt-view` de la siguiente forma:

```
virt-view -c qemu:///system prueba1
```

## Red

Como comentábamos en el punto anterior, la máquina que hemos creado se conecta, por defcto, a la red `default`. Esta red es de tipo NAT, y comprobamos que la máquina ha recibido una IP de forma dinámica y que su puerta de enlace corresponde a la dirección IP `192.168.122.1`, que corresponde con el host, el servidor DNS corresponde a la misma IP y comprobamos que tiene resolución y acceso a internet:

![características](img/caracteristica1.png)
