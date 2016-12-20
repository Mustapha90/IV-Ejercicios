# Tema 5. Virtualización completa: uso de máquinas virtuales

##Ejercicio 1
**Instalar los paquetes necesarios para usar KVM. Se pueden seguir estas instrucciones. Ya lo hicimos en el primer tema, pero volver a comprobar si nuestro sistema está preparado para ejecutarlo o hay que conformarse con la paravirtualización.**

Para instalar los paquetes necesarios para usar KVM ejecutamos la orden:

``sudo apt-get install qemu-kvm libvirt-bin``

Para usar ``KVM`` con un usuario que no es el root, hay que añadir el usuario a los grupos ``kvm`` y ``libvirt``

``$ adduser <usuario> kvm``

``$ adduser <usuario> libvirt``

Para comprobar si el sistema está preparado para ejecutar KVM usamos el comando ``kvm-ok``

``$ kvm-ok``

![Imagen 5-1](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema5-1.png)

##Ejercicio 2
**1. Crear varias máquinas virtuales con algún sistema operativo libre tal como Linux o BSD. Si se quieren distribuciones que ocupen poco espacio con el objetivo principalmente de hacer pruebas se puede usar CoreOS (que sirve como soporte para Docker) GALPon Minino, hecha en Galicia para el mundo, Damn Small Linux, SliTaz (que cabe en 35 megas) y ttylinux (basado en línea de órdenes solo).**

**SliTaz**

Descargamos la imagen

Creamos un disco duro:

``$ qemu-img create -f qcow2 slitaz_hd.qcow2 2G`` 

Creamos la máquina virtual con el disco duro creado y la imagen descargada:

``qemu-system-x86_64 -machine accel=kvm -hda slitaz_hd.qcow2 -cdrom slitaz-4.0.iso -m 1G -boot d``

**SliTaz**

Descargamos la imagen

Creamos un disco duro:

``$ qemu-img create -f qcow2 slitaz_hd.qcow2 2G`` 

Creamos la máquina virtual con el disco duro creado y la imagen descargada:

``qemu-system-x86_64 -machine accel=kvm -hda slitaz_hd.qcow2 -cdrom slitaz-4.0.iso -m 1G -boot d``

![Imagen 5-2](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema5-2.png)

**ttylinux**

Creamos el disco duro:

``qemu-img create -f qcow2 ttylinux_hd.qcow2 2G``

Lanzamos la MV:

``qemu-system-x86_64 -machine accel=kvm -hda ttylinux_hd.qcow2 -cdrom ttylinux-pc_x86_64-2015.02.iso -m 1G -boot d``


![Imagen 5-3](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema5-3.png)

**2. Hacer un ejercicio equivalente usando otro hipervisor como Xen, VirtualBox o Parallels.**

Creación de una máquina virtual de ``SliTaz`` usando VirtualBox

![Imagen 5-4](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema5-4.gif)


##Ejercicio 3
**Crear un benchmark de velocidad de entrada salida y comprobar la diferencia entre usar paravirtualización y arrancar la máquina virtual simplemente con qemu-system-x86_64 -hda /media/Backup/Isos/discovirtual.img``**




##Ejercicio 4
**Crear una máquina virtual Linux con 512 megas de RAM y entorno gráfico LXDE a la que se pueda acceder mediante VNC y ssh.**

**VNC**

He usado la imagen de Lubuntu ya que usa el entorno gráfico LXDE.

Creamos un disco duro para la MV:

``$ qemu-img create -f qcow2 lubuntu.qcow2 12G``

Arrancamos la máquina virtual y instalamos Lubuntu:

``$ qemu-system-x86_64 -machine accel=kvm -hda lubuntu.qcow2 -cdrom lubuntu-16.10-desktop-amd64.iso -m 2G -boot d``

Instalamos vinagre:

``$ sudo apt-get install vinagre``

Lanzamos la máquina virtual dentro de un servidor vnc, y con 512 megas de RAM:

``$ qemu-system-x86_64 -machine accel=kvm -hda lubuntu.qcow2 -m 512M -vnc :1``

Nos conectamos a la MV usando vinagre:

``$ vinagre localhost:1``

![Imagen 5-5](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema5-5.png)

**SSH**

Para conectarnos a la MV usando ssh la arrancamos con el siguiente comando:

``$ qemu-system-x86_64 -machine accel=kvm -hda lubuntu.qcow2 -m 512M -redir tcp:2223::22``

Instalamos `openssh-server` dentro de la máquina:

``$ sudo apt-get install openssh-server``

Ahora podemos conectarnos a la máquina con ssh:

``$ ssh -p 2223 lubuntu@localhost``

![Imagen 5-6](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema5-6.png)

##Ejercicio 5
**Crear una máquina virtual ubuntu e instalar en ella alguno de los servicios que estamos usando en el proyecto de la asignatura.**



##Ejercicio 6
**Instalar una máquina virtual con Linux Mint para el hipervisor que tengas instalado.**

Creamos un disco para la máquina:

``$ qemu-img create linuxmint.img 12G``

Lanzamos la máquina y instalamos el sistema operativo:

``$ qemu-system-x86_64 -machine accel=kvm -hda linuxmint.img -cdrom linuxmint-18.1-mate-64bit.iso -m 2G -boot d``

Después de la instalación arrancamos la máquina desde el disco duro:

``$ qemu-system-x86_64 -machine accel=kvm -hda linuxmint.img -m 2G``
 
![Imagen 5-7](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema5-72.png)


