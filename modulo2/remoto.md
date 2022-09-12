# Conexión remota a libvirt

Podemos conectar al hypervirsor libvirt que se está ejecutando en un servidor desde otra máquina remota. Es decir, desde una máquina cliente podemos usar, por ejmplo, `virsh` para conectarnos a un hypervisor libvirt remoto. Para ello, la sintaxis es la siguiente:

```
usuario@cliente:~$ virsh -c qemu+ssh://usuario_remoto@ip_servidor_remoto/system comando
```

Es decir, se va a producir una conexión ssh entre la máquina cliente y el servidor donde se ejecuta libvirt. Por lo tanto hay que configurar las máquinas para que el usuario `usuario` de la máquina `cliente` pueda acceder por SSH al servidor remoto con el usuario `usuario_remoto`, sin que se se le pida la contraseña.

Para ello tendremos que copiar la clave pública SSH del usuario `cliente` al fichero `~/.ssh/authorized_keys` del usuario `usuario_remoto` en el servidor que queremos acceder.

---

[Índice](https://github.com/josedom24/curso_virtualizacion_linux)
