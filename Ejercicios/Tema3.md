# Tema 3. Creando aplicaciones en la nube: Uso de PaaS

##Ejercicio 1
**Darse de alta en algún servicio PaaS tal como Heroku, Nodejitsu, BlueMix u OpenShift.**

Me he dado de alta en Hroku y OpenShift para probar los dos.

**Heroku**

![Imagen 1](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema31.png)


**OpenShift**

![Imagen 2](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema32.png)

##Ejercicio 2
**Crear una aplicación en OpenShift o en algún otro PaaS en el que se haya dado uno de alta. Realizar un despliegue de prueba usando alguno de los ejemplos.**

Entramos a nuestra cuenta en OpenShift, creamos un proyecto, y le añadimos una aplicación eligiendo ``nodejs:4`` como entorno.

![Imagen 3](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema33.png)

Introducimos el repositorio de la aplicación y un nombre

![Imagen 4](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema34.png)

Ahora OpenShift va a realizar la construcción y el despliegue de la aplicación

![Imagen 5](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema35.png)

Ya podemos acceder a la aplicación mediante el enlace que nos ha facilitado OpenShift

![Imagen 6](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema36.png)


##Ejercicio 3
**Realizar una app en express (o el lenguaje y marco elegido) que incluya variables como en el caso anterior.**

He creado una simple API REST en Python Flask que permite obtener el prefijo telefónico de un país:

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

from flask import Flask, jsonify, abort, request, make_response, url_for

app = Flask(__name__, static_url_path = "")

paises = [
    {
        'nombre': 'brazil',
        'codigo': u'55',
    },
    {
        'nombre': 'france',
        'codigo': u'33',
    },
    {
        'nombre': 'italy',
        'codigo': u'39',
    },
    {
        'nombre': 'japan',
        'codigo': u'81',
    },
]


@app.route("/")
def home():
    return """
    <html>
    <body> 
        <h2>Bienvenido a la página principal de la API de Prefijos telefónicos mundiales:</h2>
        <h4>Uso:</h4>
        <h5>Mostrar todos los paises:</h5>
        <p>/api/paises<p>
        <h5>Mostrar un pais:</h5>
        <p>/api/paises/"pais"</p>
        <p>Ej: /api/paises/france</p>
    </body> 
</html> """ 


def listar_paises(pais):
    nuevo_pais = {}
    for campo in pais:
        if campo == 'nombre':
            nuevo_pais['uri'] = url_for('get_pais', nombre_pais = pais['nombre'], _external = True)
        else:
            nuevo_pais[campo] = pais[campo]
    return nuevo_pais
    
@app.route('/api/paises', methods = ['GET'])
def get_paises():
    return jsonify( { 'paises': map(listar_paises, paises) } )

@app.route('/api/paises/<nombre_pais>', methods = ['GET'])
def get_pais(nombre_pais):
    pais = filter(lambda p: p['nombre'] == nombre_pais, paises)
    if len(pais) == 0:
        abort(404)
    return jsonify( { 'pais': listar_paises(pais[0]) } )

@app.errorhandler(400)
def not_found(error):
    return make_response(jsonify( { 'error': 'Bad request' } ), 400)

@app.errorhandler(404)
def not_found(error):
    return make_response(jsonify( { 'error': 'Not found' } ), 404)
    
if __name__ == '__main__':
    app.run(host='0.0.0.0', debug = True)
```

##Ejercicio 4
**Crear pruebas para las diferentes rutas de la aplicación.**

Usando la librería unittest de Python se han realizado los siguientes tests:

```python
from app import app

import os
import json
import unittest
import tempfile

paises = [
    {
        'nombre': 'brazil',
        'codigo': u'55',
    },
    {
        'nombre': 'france',
        'codigo': u'33',
    },
    {
        'nombre': 'italy',
        'codigo': u'39',
    },
    {
        'nombre': 'japan',
        'codigo': u'81',
    },
]
class flaskAppTestCase(unittest.TestCase):
    def test_home(self):
        tester = app.test_client(self)
        result = tester.get('/')
        self.assertEqual(result.status_code, 200)

    def test_paises(self):
        tester = app.test_client(self)
        response = tester.get('/api/paises', content_type='application/json')
        dic = json.loads(response.data) 
        self.assertEqual(response.status_code, 200)
        self.assertEqual(len(dic['paises']),len(paises))

    def test_pais(self):
        tester = app.test_client(self)
        result = tester.get('/api/paises/brazil')
        self.assertEqual(result.status_code, 200)

if __name__ == '__main__':
    unittest.main()
```

Ejecutamos los tests con la siguiente orden:

``python tests.py``

![Imagen 7](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema37.png)

##Ejercicio 5
**Instalar y echar a andar tu primera aplicación en Heroku.**

Para hacer este ejercicio he seguido los pasos explicados en el tema 3.

Primero instalamos el toolbelt de heroku

``$ wget -O- https://toolbelt.heroku.com/install-ubuntu.sh | sh``

Entramos a nuestra cuenta con:

``$ heroku login``

Descargamos la aplicación de ejemplo para node:

``$ git clone git@github.com:heroku/node-js-getting-started.git``

Cambiamos al directorio de la aplicación descargada:

``$ cd node-js-getting-started/``

Creamos la aplicación:

``$ heroku create``

Y por último desplegamos la aplicación con un push a la rama master de heroku:

``$ git push heroku master``

![Imagen 8](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema38.png)

Accedemos a la URL indicada en la captura anterior para visualizar la aplicación:

![Imagen 9](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema39.png)


##Ejercicio 6
**Usar como base la aplicación de ejemplo de heroku y combinarla con la aplicación en node que se ha creado anteriormente. Probarla de forma local con foreman. Al final de cada modificación, los tests tendrán que funcionar correctamente; cuando se pasen los tests, se puede volver a desplegar en heroku.**

Instalamos foreman:

``$ gem install foreman``

Creamos un fichero Procfile con el siguiente contenido:

```
web: gunicorn app:app --log-file=-
```

Creamos un fichero para las dependencias ``requirements.txt``

```
Flask
gunicorn
```

Lanzamos ``foreman``

``$ foreman start``

![Imagen 10](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema310.png)

Visualizamos la aplicación en el navegador:

![Imagen 11](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema311.png)

##Ejercicio 7
**Haz alguna modificación a tu aplicación en node.js para Heroku, sin olvidar añadir los tests para la nueva funcionalidad, y configura el despliegue automático a Heroku usando Snap CI o alguno de los otros servicios, como Codeship, mencionados en StackOverflow**

Para configurar el despliegue automático junto con la integración continua, vamos a seguir los siguientes pasos:

Primero, necesitamos 4 ficheros:

**Procfile** para que heroku pueda lanzar la aplicación

```
web: gunicorn app:app --log-file=-
```

**requirements.txt** contiene las dependencias de la aplicación

```
Flask
gunicorn
```
**.travis.yml** para la integración continua con Travis-CI

```
language: python
python:
  - "2.7"

# instalar las dependencias
install: 
 - pip install -r requirements.txt


# ejecutar tests
script: 
 - python tests.py
```

**runtime.txt** contiene la versión de python usada

```
python-2.7.12
```

Ahora nos vamos al dashboard de heroku para crear la aplicación.

Nos conectamos con el repositorio github de nuestra aplicación, activamos la opción de espera de integración continua y la del despliegue automático:

![Imagen 12](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema312.png)

A partir de ahora, cualquier push a la rama master del repositorio usando git, inicia la integración continua y el delspliegue automáticamente.

![Imagen 13](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema313.png)


##Ejercicio 8
**Preparar la aplicación con la que se ha venido trabajando hasta este momento para ejecutarse en un PaaS, el que se haya elegido.**

Los pasos para realizar el depliegue están explicados en el ejercicio anterior

[repositorio de la aplicación](https://github.com/Mustapha90/APIRestFlask)

Despliegue en Heroku: [https://ivtema3.herokuapp.com/](https://ivtema3.herokuapp.com/)

