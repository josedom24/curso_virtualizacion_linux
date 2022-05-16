# Creación de máquinas virtuales con virt-install

Vamos a crear nuestra primera máquina virtual desde la línea de comandos con la aplicación `virt-install`.

Lo primero que tenemos que hacer es instalar el paquete `virtinst`, que además de este programa, tiene otras utilidades que iremos usando a los largo del curso.

```
apt install virtinst
```

La información que tenemos que proporcionar a `virt-install` para la creación de la nueva máquina virtual será la siguiente:

* El nombre de la máquina virtual (parámetro `--name`).
* El tipo de virtualización (parámetro --virt-type). en nuestro caso será `kvm`.
* En nuestro caso vamos a realizar una instalación desde un fichero ISO, por lo que tendremos que indicar que la nueva máquina tendrá un CDROM con la ISO que indiquemos (parámetro `--cdrom`).
* La variante del sistema operativo que vamos a utilizar (parámtreo `--os-variant`). Para obtener la lista de variantes de sistemas operativos, podemos ejecutar `osinfo-query os`. 
* El tamaño del disco (parámetro `--disk size`). Se creará un fichero con la imagen del disco que se guardará en `/var/lib/libvirt/images`.
* La cantidad de memoria RAM (parámetro `--memory`)

Podemos indicar muchos más parámetros a la hora de crear la nueva máquina. Puedes obtener toda la infromación en la [documentación oficial](https://linux.die.net/man/1/virt-install) de la aplicación. Iremos usando, a lo largo del curso, diferentes parámetros de esta herramienta.

## Creación de nuestra primera máquina virtual.

Vamos a crear una máquina con las siguientes características: se va a llamar `prueba1`, se va a usar una ISO de la distribución GNU/Linux Debian 11, la variante de sistema operativo podemos poner `debian10`, el tamñao del disco será de 10Gb y la memoria RAM será de 1GB. No vamos a indicar la red a la que se conecta ya que, por defecto, se conectará a la red predefinida `default`.

Tenemos que tener en cuenta dos cosas:

1. La red `default` debe estar activa: `virsh -c qemu:///system net-start default`.
2. Hemos bajado una imagen ISO para la instalación del sistema operativo y la tenemos guardad en el directorio `~/ISO`.

Para crear la nueva máquina con esas características, ejecutamos:

virt-install --virt-type kvm \
			 --name prueba1 \
			 --cdrom ~/iso \
			 --os-variant debian10 \
			 --disk size=10 
			 --memory 1024

		
		
