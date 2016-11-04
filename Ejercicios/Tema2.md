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


##Ejercicio 3.
**Ejecutar el programa en diferentes versiones del lenguaje. ¿Funciona en todas ellas?**

La aplicación ha sido creada con python 3.4.3, la vamos a ejecutar con Python 2.7.6:

![Imagen 3](http://i1210.photobucket.com/albums/cc420/mj4ever001/imagen3.png)

Como se puede observar, la aplicación funciona correctamente.

##Ejercicio 4.
**Crear una descripción del módulo usando package.json. En caso de que se trate de otro lenguaje, usar el método correspondiente.**

El equivalente en Python es definir el fichero setup.py, una posible solución es la siguiente:

```python
from setuptools import find_packages, setup

setup(
    name='AppEncuestas',
    version='0.1',
    packages=find_packages(),
    include_package_data=True,
    license='GPL-3.0',
    description='Aplicación web simple que permite publicar encuestas',
    url='https://github.com/Mustapha90/AppEncuestas',
    author='Mustapha Mayo',
    author_email='mustapha@correo.ugr.es',
    classifiers=[
        'Environment :: Web Environment',
        'Framework :: Django',
        'Framework :: Django :: 1.10.2',  
        'Intended Audience :: Developers',
        'License :: OSI Approved :: GPL-3.0', 
        'Operating System :: OS Independent',
        'Programming Language :: Python :: 3.4.3',
        'Topic :: Internet :: WWW/HTTP',
        'Topic :: Internet :: WWW/HTTP :: Dynamic Content',
    ],
)
```

##Ejercicio 5.
**Automatizar con grunt, gulp u otra herramienta de gestión de tareas en Node la generación de documentación de la librería que se cree usando docco u otro sistema similar de generación de documentatión. . Previamente, por supuesto, habrá que documentar tal librería.**

Para python se puede usar epydoc, lo instalamos con el siguiente comando:

``$ pip install epydoc``

Como ejemplo de su uso hemos documentado el fichero de vistas ``views.py``

Ahora ejecutamos epydoc:

``epydoc polls/views.py``

En la carpeta html generada por epydoc podemos encontrar el fichero index.html que contiene la documentación.

![Imagen 4](http://i1210.photobucket.com/albums/cc420/mj4ever001/imagen4.png)


##Ejercicio 6.
**Para la aplicación que se está haciendo, escribir una serie de aserciones y probar que efectivamente no fallan. Añadir tests para una nueva funcionalidad, probar que falla y escribir el código para que no lo haga (vamos, lo que viene siendo TDD).**

Creamos un Test para comprobar que las encuestas se crean correctamente.

En el fichero ``tests.py`` añadimos el siguiente código:

```python
from .models import Question
from datetime import datetime

from django.test import TestCase

class EncuestaTest(TestCase):

    def setUp(self):
        encuesta="¿cuál es tu color favorito?"
        now = datetime.now()
        self.poll = Question.objects.create(question_text=encuesta, pub_date=now)
        self.poll.choice_set.create(choice_text="Rojo", votes=0)
        self.poll.choice_set.create(choice_text="Azul", votes=0)
        self.poll.choice_set.create(choice_text="Verde", votes=0)

    def test_models(self):
        self.assertEqual(self.poll.choice_set.all().count(), 3)
```

Ejecutamos el Test:

``python manage.py test polls``

![Imagen 6](http://i1210.photobucket.com/albums/cc420/mj4ever001/imagen6.png)

Ahora creamos un test de una funcionalidad que no está dsarrollada todavía, es una simple funcionalidad que hace que la url principal http://127.0.0.1:8000/ redireccione a nuestra aplicación http://127.0.0.1:8000/polls/ en vez de mostrar el mensaje "Page not found (404)".

```python
class TestRedir(TestCase):
    def test_redir(self):
        response = self.client.get('/')
        self.assertRedirects(response, '/polls/', status_code=302)
```

Ejecutamos el test:

![Imagen 61](http://i1210.photobucket.com/albums/cc420/mj4ever001/imagen61.png)

Como se puede ver en la imagen el test falla, para implementar esta simple funcionalidad editamos el fichero urls.py:

```python
from django.conf.urls import include, url
from django.contrib import admin
# imporamos el siguiente modulo
from django.views.generic.base import RedirectView

urlpatterns = [
	# Redireccionamos a la ruta de nuestra aplicación
    url(r'^$', RedirectView.as_view(url='/polls/')),
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```

Volvemos a ejecutar el test:

![Imagen 62](http://i1210.photobucket.com/albums/cc420/mj4ever001/imagen62.png)

##Ejercicio 7.
**Convertir los tests unitarios anteriores con assert a programas de test y ejecutarlos desde mocha, usando descripciones del test y del grupo de test de forma correcta. Si hasta ahora no has subido el código que has venido realizando a GitHub, es el momento de hacerlo, porque lo vas a necesitar un poco más adelante.**

En Django este proceso es automático, se ponen todos los tests en el fichero ``tests.py`` de nuestra aplicación y se ejecutan con:

``python manage.py test polls``


##Ejercicio 8.
**Haced los dos primeros pasos antes de pasar al tercero.**

Creamos el archivo ``.travis.yml`` lo añadimos a nuestro repositorio.

* **Contenido del archivo ``.travis.yml``**

```
language: python
python:
 - "3.4.3"

install:
 - sudo apt-get install python-dev
 - pip install --upgrade pip
 - pip install Django

script:
 - python manage.py test 
```

La integración funciona correctamente:
[![Build Status](https://travis-ci.org/Mustapha90/AppEncuestas.svg?branch=master)](https://travis-ci.org/Mustapha90/AppEncuestas)



