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


#### Ejercicio 4 ####

Instalar algun sistema *debianita* y configurarlo para su uso. Trabajando desde terminal, probar a ejecutar alguna 
aplicación  o instalar las herramientas necesarias para compilar una y ejecutarla.

Instalo el sistema de la siguiente forma:

> sudo debootstrap --arch=amd64 wheezy /home/jaulas/debian http://ftp.debian.org/debian/

Una vez instalado accedemos al sistema con la siguiente orden:

> sudo chroot /home/jaulas/debian/

Listamos su contenido para comprobar que todo se ha instalado correctamente. Después intentamos ejecutar la aplicación
top, por ejemplo:

![imagen3](https://dl.dropbox.com/s/c8qmpbp5u5qj4bt/Captura%20de%20pantalla%20de%202013-11-04%2012%3A56%3A56.png)

Corregimos el error y ejecutamos *top*

> mount -t proc proc /proc

![imagen4](https://dl.dropbox.com/s/du94low4zg0kwfq/Captura%20de%20pantalla%20de%202013-11-04%2013%3A02%3A16.png)

#### Ejercicio 5 ####

Instalar una jaula chroot para ejecutar el servidor web de altas prestaciones nginx.

Accedemos a la jaula como ya sabemos:

> sudo chroot /home/jaulas/debian/

Instalamos el SWAP nginx:

> apt-get install nginx

![imagen5_1](https://dl.dropbox.com/s/slp9vje4zjzxjst/5_1.png)

Ejecutamos:

> service nginx start

![imagen5_2](https://dl.dropbox.com/s/2cf5r4vw8c9fs6p/5_2.png)

Ya esta funcionando:

> service nginx status

![imagen5_3](https://dl.dropbox.com/s/hj7ocx1iohil0vq/5_3.png)

#### Ejercicio 6 ####

Crear una jaula y enjaular un usuario usando *jailkit*, que previamente se habrá tenido que instalar.

Lo primero será instalar el paquete *jailkit*. Para ello he utilizado el siguiente material:

http://www.trey.es/blog/linux/usuarios-enjaulados-para-ssh-jailkit/

> ./configure
make
sudo make install

Antes de instalar la jaula debemos crear un sistema de ficheros poseido por root:

> sudo mkdir /home/jaula/ejercicio6
chown -R root:root /home/jaula/ejercicio6

Ahora creamos la jaula:

> sudo jk_init -v -j /home/jaula/ejercicio6/ jk_lsh basicshell netutils editors

Con la opcion -v muestra todos los mensajes y la opcion -j indica el directorio donde se encuentra la jaula.

El siguiente paso es crear un usuario y "enjaularlo":

> sudo adduser someuser

> sudo jk_jailuser -m -j home/jaula/ejercicio6/ someuser/

