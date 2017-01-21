# Tema 6. Gestión de infraestructuras virtuales

##Ejercicio 1
**Instalar chef en la máquina virtual que vayamos a usar**


##Ejercicio 2
**Crear una receta para instalar la aplicación que se viene creando en la asignatura en alguna máquina virtual o servidor en la nube.**


##Ejercicio 3
**Desplegar la aplicación de DAI con todos los módulos necesarios usando un playbook de Ansible.**


##Ejercicio 4
**Instalar una máquina virtual Debian usando Vagrant y conectar con ella.**

He instalado Ubuntu 16 Server (Xenial Xerus)	

Que se puede encontrar [aquí](https://atlas.hashicorp.com/gbarbieru/boxes/xenial)

Descargamos la maquina:

``$ vagrant box add gbarbieru/xenial https://atlas.hashicorp.com/gbarbieru/boxes/xenial``


Iniciamos la maquina:

``$ vagrant init gbarbieru/xenial``


Arrancamos la maquina:

``$ vagrant up``

Conectamos usando ssh a la máquina:

``$ vagrant ssh``

![Imagen 6-1](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema6-1.png)

##Ejercicio 5
**Crear un script para provisionar `nginx` o cualquier otro servidor web que pueda ser útil para alguna otra práctica**

Creamos un fichero Vagrantfile con el siguiente contenido:

```
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "gbarbieru/xenial"

  config.vm.provision "shell",
    inline: "sudo apt-get update && sudo apt-get install -y nginx && sudo service nginx start"

end
```

Ejecutamos el siguiente comando para provisionar la maquina:

vagrant provision

##Ejercicio 6
**Configurar tu máquina virtual usando vagrant con el provisionador chef.**
