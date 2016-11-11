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


##Ejercicio 4
**Crear pruebas para las diferentes rutas de la aplicación.**


##Ejercicio 5
**Instalar y echar a andar tu primera aplicación en Heroku.**


##Ejercicio 6
**Usar como base la aplicación de ejemplo de heroku y combinarla con la aplicación en node que se ha creado anteriormente. Probarla de forma local con foreman. Al final de cada modificación, los tests tendrán que funcionar correctamente; cuando se pasen los tests, se puede volver a desplegar en heroku.**


##Ejercicio 7
**Haz alguna modificación a tu aplicación en node.js para Heroku, sin olvidar añadir los tests para la nueva funcionalidad, y configura el despliegue automático a Heroku usando Snap CI o alguno de los otros servicios, como Codeship, mencionados en StackOverflow**


##Ejercicio 8
**Preparar la aplicación con la que se ha venido trabajando hasta este momento para ejecutarse en un PaaS, el que se haya elegido.**


