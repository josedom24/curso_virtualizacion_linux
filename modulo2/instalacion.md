# Instalación de QEMU/KVM + libvirt

Para trabajar con el sistema de virtualización QEMU/KVM + libvirt en nuestra distribución Linux Debian/Ubuntu, vamos a instalar los siguientes paquetes:

```
apt install qemu-system libvirt-clients libvirt-daemon-system
```

libvirt proporciona varios mecanismos para conectarse a un hipervisor qemu-kvm.

Podemos obtener las versiones de las aplicaciones que hemos instalado (en mi caso en una distribución GNU/Linux Debian 11), ejecutando:

```
virsh version
Compilada con biblioteca: libvirt 7.0.0
Uso de biblioteca: libvirt  7.0.0
Utilizando API: QEMU 7.0.0
Ejecutando hypervisor: QEMU 5.2.0
```

## Conexión a QEMU/KVM

Vamos a usar la utilidad `virsh`, que nos proporciona una shell completa para el manejo de libvirt. Con el comando `list` mostramos las máquinas virtuales que hemos creado.

Con un usuario sin privilegios ejecutamos:

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
usuario@kvm:~$ virsh -c qemu:///system list
```

De forma alternativa y según la [wiki de Debian](https://wiki.debian.org/es/KVM#M.2BAOE-quinas_virtuales_del_usuario_y_del_sistema), podemos usar la variable de entorno `LIBVIRT_DEFAULT_URI` con el siguiente comando:

```bash
export LIBVIRT_DEFAULT_URI='qemu:///system'
```

---

[Índice](https://github.com/josedom24/curso_virtualizacion_linux)

