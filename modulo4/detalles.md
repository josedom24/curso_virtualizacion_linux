# Detalles de las máquinas virtuales

Podemos acceder al detalle de una máquina virtual de tres formas distintas: haciendo **doble click** sobre la máquina, escogiendo la máquina y pulsando el botón **Abrir** o escogiendo la opción del menú **Editar -> Detalles de la máquina virtual**.

Vamos a abrir una ventana con las siguientes opciones:

![detalles](img/detalles1.png)

* **Archivo**: Opciones generales: ver el gestor, cerrar la ventana,...
* **Máquina Virtual**: Opciones para gestionar la máquina.
* **Vista**: Nos permite ver las distintas vistas, controlar la ventana de la consola (pantalla completa, escalar, ...). Veamos las vistas:
	* **Consola**: Accedemos a una consola donde controlamos la máquina virtual. También se accede con el **botón 1**.
	* **Detalles**: Obtenemos la configuración de la máquina virtual y los dispositivos hardware. Podemos quitar y añadir nuevos dispositivos y hacer las modificaciones necesarias.También se accede con el **botón 2**.
	* **Instantáneas**: Ventana para gestionar las instantáneas de la máquina virtual. Estudiaremos más adelante las instantáneas. También se accede con el **botón 3**.
* **Enviar Tecla**: Combinación de teclas que podemos enviar a la máquina virtual.

## Vista Consola

![detalles](img/detalles2.png)

Accedemos a una consola desde donde podemos controlar la máquina virtual. Desde el menú vista podemos configurar el tamaño de la pantalla (pantalla completa, escalar monitor, ...). Puede ser una buena herramienta para realizar pequeñas modificaciones a la máquina, pero es recomendable utilizar distintos protocolos para el acceso y gestión de la máquina virtual (SSH, VCN, RDP,...)

## Vista Detalles

![detalles](img/detalles3.png)

En esta vista se nos muestra la definición XML de la máquina virtual de forma gráfica. Además, nos posibilita hacer cambios en la configuración de la misma. Vemos la configuración general de la máquina y las características de los dispositivos hardware que tiene configurada. Podemos quitas dispositivos y añadir otros nuevos.

Veamos los elementos fundamentales:

* **Repaso**: Nos da la información general de la máquina virtual. 

![detalles](img/detalles4.png)

Además, en todo momento podemos acceder a la definición XML:

![detalles](img/detalles5.png)

* **CPUs**: Configuración de las vCPU asignadas a la máquina. Podemos modificar este valor. Si la máquina está ejecutándose la modificación será efectiva en el siguiente arranque de la máquina.

![detalles](img/detalles6.png)

* **Memoria**: Del mismo modo, vemos la configuración de asignación de memoria RAM de la máquina. Podemos modificar la **memoria actual** y la **memoria máxima**. Del mismo modo, necesitamos reiniciar la máquina para que tenga efecto el cambio.

![detalles](img/detalles7.png)

* **Opciones de arranque**: Podemos ver y configurar el orden de los dispositivos de arranque.

**Imagen de opciones de arranque**

A continuación se nos muestra los distintos dispositivos hardware que tiene configurado la máquina: unidades de disco, interfaces de red, teclado, ratón, adaptador de vídeo, interfaces, ... Pudiendo hacer también, modificaciones en los mismos. Veamos algunos de ellos:

* **Discos**: Nos da información del disco que tiene configurada la máquina. Es importante, como ya hemos indicado anteriormente, que el el driver sea VirtIO para obtener mayor rendimiento. Vemos que podemos añadir a las máquinas virtuales tantos discos como sean necesarios.

![detalles](img/detalles8.png)

* **Interfaces de red**: Obtenemos la información de las distintas interfaces de red de la máquina. en este caso también usamos VirtIO como modelo de dispositivo. Vemos a que red está conectada. Si la máquina se está ejecutando, podemos ver la dirección IP de la interfaz. Del mismo modo, los cambios serán efectivos tras el reinicio de la máquina.

![detalles](img/detalles9.png)

Por último, tenemos dos operaciones referente a los dispositivos hardware:

* Si seleccionamos uno de ellos, y pulsamos el botón derecho del ratón nos da la posibilidad de **Eliminar Hardware**.
* Con el botón **Agregar Hardware**, tenemos la posibilidad de añadir nuevos componentes a la configuración de la máquina. Hay que indicar que algunos dispositivos se pueden agregar "en caliente", con la máquina en estado de ejecución. En los próximos apartados del curso usaremos está opción para añadir nuevos componentes a nuestras máquinas virtuales.

![detalles](img/detalles10.png)


