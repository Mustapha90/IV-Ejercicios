# Tema 2. Desarrollo basado en pruebas

##Ejercicio 1
**Instalar alguno de los entornos virtuales de node.js (o de cualquier otro lenguaje con el que se esté familiarizado) y, con ellos, instalar la última versión existente, la versión minor más actual de la 4.x y lo mismo para la 0.11 o alguna impar (de desarrollo).**

He elegido [virtualenv] (https://virtualenv.pypa.io/en/latest/) que es un entorno virtual de desarrollo para Python.

Primero instalamos el gestor de paquetes de python ``python-pip``

``$ sudo apt-get update``

``$ sudo apt-get install python-pip``

Ahora usamos ``pip`` para instalar la última versión de ``virtualenv``

``$ sudo pip install virtualenv``

En el siguiente [enlace](http://docs.python-guide.org/en/latest/dev/virtualenvs/) podemos encontrar información sobre el funcionamiento de virtualenv 

Ya podemos crear entornos para diferentes versiones de python por ejemplo:

* **Para python 2.7**

``$ virtualenv -p /usr/bin/python2.7 ventorno27``

* **Para python 3**

``$ virtualenv -p /usr/bin/python3 ventorno3``

Antes de empezar a trabajar con el entorno hay que activarlo:

``$ source ventorno27/bin/activate``

Para desactivarlo ejecutamos:

``$ deactivate``

![Imagen 1](http://i1210.photobucket.com/albums/cc420/mj4ever001/cap1tema2.png)


##Ejercicio 2.
**Como ejercicio, algo ligeramente diferente: una web para calificar las empresas en las que hacen prácticas los alumnos. Las acciones serían Crear empresa Listar calificaciones para cada empresa crear calificación y añadirla (comprobando que la persona no la haya añadido ya) borrar calificación (si se arrepiente o te denuncia la empresa o algo) Hacer un ránking de empresas por calificación, por ejemplo Crear un repositorio en GitHub para la librería y crear un pequeño programa que use algunas de sus funcionalidades. Si se quiere hacer con cualquier otra aplicación, también es válido. Se trata de hacer una aplicación simple que se pueda hacer rápidamente con un generador de aplicaciones como los que incluyen diferentes marcos MVC. Si cuesta mucho trabajo, simplemente prepara una aplicación que puedas usar más adelante en el resto de los ejercicios.**

He creado una pequeña aplicación que permite al administrador crear encuestas mediante un interfaz web, y a los usuarios realizar votos.

La aplicación se ha creado usando el framework django siguiendo este [tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial01/)

La aplicación se puede encontrar en este [Repositorio](https://github.com/Mustapha90/AppEncuestas)

![Imagen 2](http://i1210.photobucket.com/albums/cc420/mj4ever001/appencuestas.png)


##Ejercicio 3:


##Ejercicio 4:


##Ejercicio 5:


##Ejercicio 6:


##Ejercicio 7:


##Ejercicio 8:

