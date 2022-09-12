# Introducción a LXC

**LinuX Containers**, también conocido por el acrónimo **LXC**, es una tecnología de virtualización ligera o por contenedores, que es un método de virtualización en el que, sobre el núcleo del sistema operativo se ejecuta una capa de virtualización que permite que existan múltiples instancias aisladas de espacios de usuario, en lugar de solo uno. A estas instancias la llamamos **contenedores**.

Todo esto ha sido posible por el desarrollo de dos componentes del nucleo de Linux:

* Los *Grupos de Control* [cgroups](https://wiki.archlinux.org/title/Cgroups), en concreto en Debian 11 se utiliza [cgroupsv2](https://medium.com/nttlabs/cgroup-v2-596d035be4d7): que limita el uso de recursos (límite de memoria, cpu, I/O o red) para un proceso y sus hijos.
* Los *Espacios de Nombres* [namespaces](http://laurel.datsi.fi.upm.es/~ssoo/SOA/namespaces.html): que proporcionan un punto de vista diferente a un proceso (interfaces de red, procesos, usuarios, etc.).

**LXC** pertenece a los denominados contenedores de sistemas, su gestión y ciclo de vida es similar al de una máquina virtual tradicional. Está mantenido por Canonical y la página oficial es [linuxcontainers.org](https://linuxcontainers.org/).

