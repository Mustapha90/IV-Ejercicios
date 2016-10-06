# Tema 1. Introducción a la infraestructura virtual: concepto y soporte físico
##Ejercicio 1:
**Consultar en el catálogo de alguna tienda de informática el precio de un ordenador tipo servidor y calcular su coste de amortización a cuatro y siete años.**

Servidor elegido: [SERVIDOR HP PROLIANT ML350P] (http://www.dynos.es/servidor-hp-proliant-ml350p-g8-xeon-e5-2620v2-2.1ghz-8gb-sin-disco-duro-hdd-2.5-matrox-g200-4514953641990__736958-421.html)

Precio total: **1799,00€**

Precio sin IVA: **1421,21€**

Un servidor entra en la categoría de **Equipos para tratamiento de la información** por lo tanto tiene un coeficiente lineal máximo
del 25% 

[Ver Tabla de años y porcentajes de amortización para sociedades a partir de 2015] (https://ayuda.cuentica.com/tabla-anos-y-porcentajes-de-amortizacion-sociedades-a-partir-de-2015/)

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

Hacemos la comparación con un [servidor dedicado] (https://asmallorange.com/hosting/dedicated/) de la compañía [A small orange] (www.asmallorange.com) y una maquina virtual en la nube de Microsoft Azure, ambos tienen caracteristicas similares, (estos son los más similares que he podido encontrar)

* **Maquina virtual de Microsoft Azure:**

![Imagen 21](https://github.com/Mustapha90/IV-Ejercicios/blob/master/Ejercicios/Capturas/Tema1/imagen21.png)

* **Servidor dedicado de "A small orange"**

![Imagen 22](https://github.com/Mustapha90/IV-Ejercicios/blob/master/Ejercicios/Capturas/Tema1/imagen22.png)

* **Comparativa:**

|    Tipo servicio    |   Precio mensual €  | Precio anual € | Precio 1% de uso anual | Precio 10% de uso anual |
| ------------------- | ------------------- | -------------- |------------------------| ------------------------|
|   VPS Azure         |        75,29        |     1806,96    |        18,06           |         180,70          |
|   Servidor dedicado |        121,122      |     1453,464   |        1453,464        |         1453,464        |

Analizamos los resultados:

El precio del servidor dedicado no cambia aunque lo usamos muy poco, mientras el servicio del Azure se paga en función del uso que se le da, la decisión de optar por uno o otro depende del uso, si no vamos a usarlo todo el tiempo es mejor optar por el servicio de Microsoft Azure, en caso contrario el servidor dedicado sale más barato.

##Ejercicio 3:
**1. ¿Qué tipo de virtualización usarías en cada caso? Comentar en el foro**

Comentado en [foro] (https://github.com/JJ/IV16-17/issues/1) 

**Comentario:**

-**Virtualización plena:** La uso cuando necesito trabajar con varios sistemas operativos, usando algún hipervisor, por ejemplo (VirtualBox, VMWare)

-**Virtualización de aplicaciones:** La uso para ejecutar aplicaciones de windows en Linux usando el paquete WINE

-**Virtualización a nivel de sistema operativo:** Cuando es necesario tener múltiples servidores virtuales aislados y seguros en un solo servidor físico, ejemplos: Linux-VServer, Virtuozzo, OpenVZ, Solaris Containers y FreeBSD Jails.



**2. Crear un programa simple en cualquier lenguaje interpretado para Linux, empaquetarlo con CDE y probarlo en diferentes distribuciones.**

He creado un programa en python que calcula la raíz cuadrada de un número:

```python
num = float(input('Introduzca un numero: '))
num_sqrt = num ** 0.5
print('La raiz cuadrada de %0.3f es %0.3f'%(num ,num_sqrt))
```

Primero, instalamos cde:

``sudo apt-get install cde``

Empaquetamos el programa en Ubuntu 14.04:

``cde python prog.py``

La orden anterior genera una carpeta con el nombre "cde-package", copiamos esa carpeta a la máquina virtual de Kali Linux 2016.2 para probar el programa que hemos empaquetado:

Nos situamos en la carpeta donde se ha empaquetado el programa, (la ruta generada por cde) , y ejecutamos la siguiente orden 

``./python.cde prog.py``

![Imagen 32](https://github.com/Mustapha90/IV-Ejercicios/blob/master/Ejercicios/Capturas/Tema1/imagen32.png)

##Ejercicio 4:
**Comprobar si el procesador o procesadores instalados tienen estos flags. ¿Qué modelo de procesador es? ¿Qué aparece como salida de esa orden?**

Ejecutamos la orden ``egrep '^flags.*(vmx|svm)' /proc/cpuinfo``

Salida:

![Imagen 4](https://github.com/Mustapha90/IV-Ejercicios/blob/master/Ejercicios/Capturas/Tema1/imagen4.png)

Lo que nos dice que el procesador tiene la virtualización a nivel de hardware activada.

Para averiguar el modelo del procesador ejecutamos la orden:

``cat /proc/cpuinfo``

El modelo del procesador es: 

``model name: Intel(R) Core(TM)2 Duo CPU     T9800  @ 2.93GHz``



##Ejercicio 5:
**1.Comprobar si el núcleo instalado en tu ordenador contiene este módulo del kernel usando la orden kvm-ok.**

Primero instalamos cpu-checker:

``sudo apt-get install cpu-checker``

Una vez instalado el programa, ejecutamos ``kvm-ok``

![Imagen 51](https://github.com/Mustapha90/IV-Ejercicios/blob/master/Ejercicios/Capturas/Tema1/imagen51.png)

La salida nos dice que el núcleo instalado contiene el módulo KVM.

**2.Instalar un hipervisor para gestionar máquinas virtuales, que más adelante se podrá usar en pruebas y ejercicios.**
El hipervisor elegido es VirtualBox, (ya lo tenía instalado)

![Imagen 52](https://github.com/Mustapha90/IV-Ejercicios/blob/master/Ejercicios/Capturas/Tema1/imagen52.png)

