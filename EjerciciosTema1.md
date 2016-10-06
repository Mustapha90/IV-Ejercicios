# Tema 1. Introducción a la infraestructura virtual: concepto y soporte físico
##Ejercicio 1:
**Consultar en el catálogo de alguna tienda de informática el precio de un ordenador tipo servidor y calcular su coste de amortización a cuatro y siete años.**

Servidor elegido: [SERVIDOR HP PROLIANT ML350P] (http://www.dynos.es/servidor-hp-proliant-ml350p-g8-xeon-e5-2620v2-2.1ghz-8gb-sin-disco-duro-hdd-2.5-matrox-g200-4514953641990__736958-421.html)
Precio total: **1799,00€**
Precio sin IVA: **1421,21€**

Un servidor entra en la categoría de **Equipos para tratamiento de la información** por lo tanto tiene un coeficiente lineal máximo
del 25% [Tabla de años y porcentajes de amortización para sociedades a partir de 2015] (https://ayuda.cuentica.com/tabla-anos-y-porcentajes-de-amortizacion-sociedades-a-partir-de-2015/)

* **Amortización a 4 años**

|    Año     | Precio sin IVA (€) | Porcentaje |   Total (€)  |
| ---------- | ------------------ | ---------- | ------------ |
|     1°     |       1421,21      |    25%     |    355,3025  |
|     2°     |       1421,21      |    25%     |    355,3025  |
|     3°     |       1421,21      |    25%     |    355,3025  |
|     4°     |       1421,21      |    25%     |    355,3025  |



* **Amortización a 7 años**

|    Año     |   Precio sin IVA | Porcentaje | Total en (€) |
| ---------- | -----------------| ---------- | ------------ |
|    1°      |  1421,21€        |    25%     |  355,3025    |
|    2°      |  1421,21€        |    25%     |  355,3025    |
|    3°      |  1421,21€        |    15%     |  213,1815    |
|    4°      |  1421,21€        |    15%     |  213,1815    |
|    5°      |  1421,21€        |    10%     |   142,121    |
|    6°      |  1421,21€        |     5%     |   71,0605    |
|    7°      |  1421,21€        |     5%     |   71,0605    |

##Ejercicio 2:
**Usando las tablas de precios de servicios de alojamiento en Internet y de proveedores de servicios en la nube, Comparar el coste durante un año de un ordenador con un procesador estándar (escogerlo de forma que sea el mismo tipo de procesador en los dos vendedores) y con el resto de las características similares (tamaño de disco duro equivalente a transferencia de disco duro) en el caso de que la infraestructura comprada se usa sólo el 1% o el 10% del tiempo.**



##Ejercicio 3:
**1. ¿Qué tipo de virtualización usarías en cada caso? Comentar en el foro**



**Crear un programa simple en cualquier lenguaje interpretado para Linux, empaquetarlo con CDE y probarlo en diferentes distribuciones.**



##Ejercicio 4:
**Comprobar si el procesador o procesadores instalados tienen estos flags. ¿Qué modelo de procesador es? ¿Qué aparece como salida de esa orden?**

Ejecutamos la orden ``egrep '^flags.*(vmx|svm)' /proc/cpuinfo``

Salida:

![Imagen 4](enlace)

Lo que nos dice que el procesador tiene la virtualización a nivel de hardware activada.

Para averiguar el modelo del procesador ejecutamos la orden:

``cat /proc/cpuinfo``

El modelo del procesador es: 

``model name	: Intel(R) Core(TM)2 Duo CPU     T9800  @ 2.93GHz``



##Ejercicio 5:
**1.Comprobar si el núcleo instalado en tu ordenador contiene este módulo del kernel usando la orden kvm-ok.**

Primero instalamos cpu-checker:

``sudo apt-get install cpu-checker``

Una vez instalado el programa, ejecutamos ``kvm-ok``

![Imagen 51](enlace)

La salida nos dice que el núcleo instalado contiene el módulo KVM.

**2.Instalar un hipervisor para gestionar máquinas virtuales, que más adelante se podrá usar en pruebas y ejercicios.**
El hipervisor elegido es VirtualBox, (ya lo tenía instalado)

![Imagen 52](enlace)

