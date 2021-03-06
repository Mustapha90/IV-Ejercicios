# Tema 4. Virtualización ligera usando contenedores

##Ejercicio 1
**Instala LXC en tu versión de Linux favorita. Normalmente la versión en desarrollo, disponible tanto en GitHub como en el sitio web está bastante más avanzada; para evitar problemas sobre todo con las herramientas que vamos a ver más adelante, conviene que te instales la última versión y si es posible una igual o mayor a la 1.0.**

Para instalar lxc en ubuntu ejecutamos:

``$ sudo apt-get install lxc``

Antes de empezar a trabajar con lxc comprobamos si nuestro hardware soporta este tipo de tecnología:

``$ lxc-checkconfig``

![Imagen 41](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema41.png)

Todas la capacidades están ``enabled``, el hardware es compatible con lxc.

##Ejercicio 2
**Comprobar qué interfaces puente se han creado y explicarlos.**

Creamos un contenedor:

``$ sudo lxc-create -t ubuntu -n micontenedor``

Arrancamos el contenedor que acabamos de crear:

``$ sudo lxc-start -n micontenedor``

![Imagen 42](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema42.png)


Comprobamos qué interfaces se han creado:

``$ ifconfig -a``

![Imagen 43](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema43.png)

Se han creado dos interfaces nuevas ``lxcbr0`` y ``vethCR24DJ`` que se usan para dar acceso a internet al contenedor y facilitar la comunicación con el anfitrión.

##Ejercicio 3
**1. Crear y ejecutar un contenedor basado en Debian.**

Creamos el contenedor:

``sudo lxc-create -t debian -n caja-debian``

Lo ejecutamos con:

``$ sudo lxc-start -n caja-debian``

![Imagen 44](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema44.png)

**2. Crear y ejecutar un contenedor basado en otra distribución, tal como Fedora. Nota En general, crear un contenedor basado en tu distribución y otro basado en otra que no sea la tuya. Fedora, al parecer, tiene problemas si estás en Ubuntu 13.04 o superior, así que en tal caso usa cualquier otra distro. Por ejemplo, Óscar Zafra ha logrado instalar Gentoo usando un script descargado desde su sitio, como indica en este comentario en el issue.**

Estoy usando Ubuntu, he instalado un contenedor de CentOS:

Para crear el contenedor es necesario instalar el gestor de paquetes ``yum``

``sudo apt-get install yum``

Creamos el contenedor:

``$ sudo lxc-create -t centos -n caja-centos``

Arrancamos el contenedor:

``$ sudo lxc-start -n caja-centos``

![Imagen 45](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema45.png)

##Ejercicio 4
**1. Instalar lxc-webpanel y usarlo para arrancar, parar y visualizar las máquinas virtuales que se tengan instaladas.**

Lo instalamos con el siguiente comando:

``sudo wget https://lxc-webpanel.github.io/tools/install.sh -O - | sudo bash``

![Imagen 46](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema46.png)

**2. Desde el panel restringir los recursos que pueden usar: CPU shares, CPUs que se pueden usar (en sistemas multinúcleo) o cantidad de memoria.**

En el panel de lxc seleccionamos un contenedor, nos sale una página donde podemos configurar los recursos del contenedor:

![Imagen 47](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema47.png)

##Ejercicio 5
**Comparar las prestaciones de un servidor web en una jaula y el mismo servidor en un contenedor. Usar nginx.**



##Ejercicio 6
**Instalar docker.**

He instalado docker siguiendo la [documentación oficial](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

Iniciamos docker como servicio:

``$ sudo docker -d &``

Arrancamos la imagen oficial de prueba:

``$ sudo docker run hello-world``

![Imagen 52](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema52.png)

##Ejercicio 7
**1. Instalar a partir de docker una imagen alternativa de Ubuntu y alguna adicional, por ejemplo de CentOS.**

Iniciamos docker como servicio:

``$ sudo docker -d &``

Descargamos la imagen de ubuntu

``$ sudo docker pull ubuntu``

Descargamos la imagen de CentOS

``$ sudo docker pull centos:latest``

Para arrancar un contenedor ejecutamos:

``$ sudo docker run -i -t <imagen>``

**2. Buscar e instalar una imagen que incluya MongoDB.**

He instalado la imagen bitnami/mongodb:latest, ``latest`` siempre refiere a la última versión:

``$ sudo docker pull bitnami/mongodb:latest``

Para listar todas las imagenes que tenemos instaladas ejecutamos:

``$ sudo docker images``

![Imagen 49](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema49.png)

##Ejercicio 8
**Crear un usuario propio e instalar nginx en el contenedor creado de esta forma.**

He usado la imagen ubuntu:trusty, los pasos que hay que realizar son los siguientes:

Arrancamos la imagen:

``$ sudo docker run -i -t ubuntu:trusty``

Creamos un usuario nuevo y le asignamos una contraseña

``$ useradd user``

``$ passwd user``

Añadimos el usuario al grupo de ``sudoers``

``$ adduser user sudo``

Iniciamos sesión con el usuario creado:

``$ su user``

Instalamos nginx y curl

``$ sudo apt-get update && sudo apt-get install nginx curl``

Iniciamos el servicio nginx

``$ sudo service nginx start``

Ejecutamos curl para probar su funcionamiento:

``$ sudo curl 127.0.01``

![Imagen 50](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema50.png)

##Ejercicio 9
**Crear a partir del contenedor anterior una imagen persistente con commit.**

Primero obtenemos el identificador del contenedor:

``$ sudo docker ps``

Guardamos el estado del contenedor en una imagen nueva con un commit:

``$ sudo docker commit e5947acf633f ubuntu_trusty_con_nginx_curl``

Comprobamos que la imagen se ha guardado correctamente:

``$ sudo docker images``

![Imagen 51](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema51.png)

##Ejercicio 10
**Crear una imagen con las herramientas necesarias para el proyecto de la asignatura sobre un sistema operativo de tu elección.**

He creado un fichero ``Dockerfile`` para construir la imagen:

```
FROM ubuntu:14.04
MAINTAINER Mustapha Mayo <mj4ever001@gmail.com>

#Variable de entorno que indica que estamos en docker
ENV EN_DOCKER=true

#Instalar dependencias
RUN sudo apt-get -y update
RUN sudo apt-get install -y git
RUN sudo apt-get install -y build-essential python-setuptools python-dev libpq-dev
RUN sudo easy_install pip
RUN sudo pip install --upgrade pip

#Clonar el repositorio del proyecto
RUN sudo git clone https://github.com/Mustapha90/IV16-17.git

#Cambiar el directorio de trabajo al directorio del proyecto
WORKDIR IV16-17

#Instalar dependencias del proyecto
RUN make install_prod

#Permitir el acceso externo al puerto 8000 que será usado por la aplicación
EXPOSE 8000

#Punto de entrada, este script se ejecuta automáticamente al iniciar el contenedor
ENTRYPOINT ["./docker_entrypoint.sh"]
```

El siguiente comando crea la imagen usando el fichero ``Dockerfile`` que se encuentra en el directorio actual:

``sudo docker build -t imagen-proyecto .``

![Imagen 60](http://i1210.photobucket.com/albums/cc420/mj4ever001/imgtema4.png)







