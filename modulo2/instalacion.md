# Instalación de QEMU/KVM + libvirt

Para trabajar con el sistema de virtualización QEMU/KVM + libvirt en nuestra distribución Linux Debian/Ubuntu, vamos a instalar los siguientes paquetes:

```
apt install qemu-system libvirt-clients libvirt-daemon-system
```

libvirt proporciona varios mecanismos para conectarse a un hipervisor qemu-kvm.

## Conexión a QEMU/KVM

Vamos a usar la utilidad `virsh` que nos proporciona una shell completa para el manejo de libvirt. Con el comando `list` mostramos las máquinas virtuales que hemos creado.

Si con un usuario sin privilegios ejecutamos:

```
usuario@kvm:~$ virsh list
```

Estaríamos haciendo una conexión local con un usuario no privilegiado (estaríamos conectando con la URI `qemu:///session` y estaríamos mostrando las máquinas virtuales de este usuario.

Si por el contrario, como `root` ejecutamos:

```
root@kvm:~# virsh list
```

Estaríamos haciendo una conexión local privilegiada (estaríamos conectando con la URI `qemu:///system`) y mostraríamos las máquinas virtuales del sistema.

Si queremos que un usuario sin privilegios pueda hacer conexiones privilegiadas, el usuario debe pertenecer el grupo `libvirt`:

```
root@kvm:~# adduser usuario libvirt
```

Para que el usuario `usuario` haga una conexión privilegiada tendrá que indicar explícitamente la conexión a la URI `qemu:///system`:

```
usuario@kvm:~$ virsh -c quemu:///system list
```

---

[Índice](https://github.com/josedom24/curso_virtualizacion_linux)

