## Tema 2 - Técnicas de virtualización ##


#### Ejercicio 1 ####

Para este ejercicio he utilizado una iso que he creado con el software libre MasterIso, que he utilizado recientemente.


Utilizamos la orden sudo unshare -m <directorio>, se usa la opción -m para indicar que estamos creando un espacio de 
nombres para unidades de montaje. A continuación creo el directorio y monto la imagen como muestro en la Captura siguiente:

![imagen1](https://dl.dropbox.com/s/80fff2m1vpoz56m/Captura%20de%20pantalla%20de%202013-10-25%2010%3A11%3A06.png)


#### Ejercicio 2 ####

1.- Mostrar los puentes configurados en el sistema operativo.

Para mostrar los puentes que tenemos configurados es nuestro SO, introducimos por la terminal la siguiente orden, una vez
instalado el paquete *bridge-utils* :


![imagen2](https://dl.dropbox.com/s/idg5lsm5dbaqugh/Captura%20de%20pantalla%20de%202013-10-25%2010%3A58%3A15.png)


2.- Crear un interfaz virtual y asignarlo al interfaz de la tarjeta wifi, si se tiene, o del fijo, si no se tiene.

Instalamos el paquete siguiente:

> sudo apt-get install bridge-utils

Creamos el puente con la opción como se indica:

> sudo brctl addbr alcantara

Lo asignamos a la interfaz de la tarjeta wifi mediante la orden:

> brctl addif alcantara eth0

Mostramos nuestras las interfaces con la siguiente orden:

> ip addr show


#### Ejercicio 3 ####

1. Usar debootstrap (o herramienta similar en otra distro) para crear un sistema mínimo que se pueda ejecutar más 
adelante.

Lo primero que haremos será instalar el paquete *debootstrap*:

> sudo apt-get install debootstrap

El siguiente paso es crearlo con la siguiente orden: (32bits)

> sudo debootstrap --arch=i386 quantal /home/jaulas/quantal/ http://archive.ubuntu.com/ubuntu


2. Experimentar con la creación de un sistema Fedora dentro de Debian usando Rinse.

Instalamos Rinse:

> sudo apt-get install rinse

> rinse --list-distributions

Esto último nos muestra las distribuciones que podemos instalar.

Ahora procedo a instalar el sistema fedora-core-4 (32bits):

> sudo mkdi /home/jaulas/fedora

> sudo rinse --arch=i386 --distribution fedora-core-4 --directory /home/jaulas/fedora


