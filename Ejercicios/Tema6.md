# Tema 6. Gestión de infraestructuras virtuales

##Ejercicio 1
**Instalar chef en la máquina virtual que vayamos a usar**

Primero configuramos ssh para poder acceder a la MV sin clave, siguiendo el siguiente [tutorial](https://docs.microsoft.com/es-es/azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys)

Nos conectamos a la máquina virtual azure con ssh

![Imagen 6-2](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema6-2.png)

Dentro de la máquina instalamos Chef

``$ curl -L https://www.opscode.com/chef/install.sh | bash``

![Imagen 6-2](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema6-3.png)

##Ejercicio 2
**Crear una receta para instalar la aplicación que se viene creando en la asignatura en alguna máquina virtual o servidor en la nube.**







##Ejercicio 3
**Desplegar la aplicación de DAI con todos los módulos necesarios usando un playbook de Ansible.**

Para realizar este ejercicio he seguido este [Tutorial](http://technivore.org/posts/2015/10/09/easy-django-deployments-with-ansible.html)

He realizado el despliegue en una máquina virtual Ubuntu 14.04, la aplicación se ha desarrollado con python y el framework Django.

He usado nginx junto con gunicorn, nginx se encargará de servir el contenido estático y funcionará como un proxy inverso, lo que merjorará mucho el rendimiento.

Se han creado los siguientes ficheros:

**hosts** Contiene el nombre del servidor donde será desplegada la app

```yml
[musiv1617]
musiv1617.cloudapp.net
```

**vars.yml** Contiene las variables necesarias para el despliegue

```yml
---

# Variables generales
project_name: practica7
install_root: /srv
wsgi_module: practica7.wsgi
project_repo: https://github.com/Mustapha90/DAI1617
static_root: "{{ install_root }}/{{ project_name }}/{{ project_name }}/static"
server_name: musiv1617.cloudapp.net www.musiv1617.cloudapp.net


# Variables de entorno de la aplicación
ON_HEROKU: *****
SECRET_KEY: : *****
MONGO_URI: : *****
API_KEY: : *****
DATABASE_URL: : *****

# Dependencias del sistema
system_packages:
  - git
  - nginx
  - gunicorn
  - python-setuptools
  - python-dev
  - build-essential
  - python-pip
  - libpq-dev
```

ON_HEROKU es una variable de entorno que indica que estamos en un entorno de producción no necesariamente en heroku.

**provision.yml** Contiene las instrucciones necesarias para el aprovisionamiento

```yml
---
- hosts: musiv1617
  vars_files:
    - vars.yml
  gather_facts: false
  become: yes

  tasks:
    - name: Instalar paquetes del sistema
      apt: pkg={{ item }} update-cache=yes
      with_items: "{{ system_packages }}"

    - name: Crear un directorio para la aplicación
      file: path={{ install_root }}/{{ project_name }} state=directory

    - name: Eliminar el fichero el sitio por defecto de nginx
      file: path=/etc/nginx/sites-enabled/default state=absent

- include: deploy.yml
```

**deploy.yml** Se encarga de realizar el despliegue

```yml
---
- hosts: musiv1617
  vars_files:
    - vars.yml
  gather_facts: false
  become: yes

  environment:
    ON_HEROKU={{ ON_HEROKU }}
    SECRET_KEY={{ SECRET_KEY }}
    MONGO_URI={{ MONGO_URI }}
    API_KEY={{ API_KEY }}
    DATABASE_URL={{ DATABASE_URL }}

  tasks:
    - name: Clonar el repositorio
      git: repo={{ project_repo }} dest={{ install_root }}/{{ project_name }} accept_hostkey=yes
      notify:
      - restart gunicorn

    - name: Instalar dependencias de la app
      pip: requirements={{ install_root }}/{{ project_name }}/requirements.txt
      notify:
      - restart gunicorn

    - name: Copiar el fichero de configuración de nginx
      template: src=files/nginx.j2 dest=/etc/nginx/sites-enabled/{{ project_name }}.conf
      notify:
      - restart nginx

    - name: Django migrate - crear la base de datos
      django_manage: command=migrate app_path={{ install_root }}/{{ project_name }}

    - name: collectstatic - crear ficheros estáticos 
      django_manage: command=collectstatic app_path={{ install_root }}/{{ project_name }}

    - name: Copiar el fichero de configuración de nginx
      template: src=files/gunicorn.j2 dest=/etc/init/gunicorn.conf
      notify:
      - restart gunicorn

    - name: Comprobar que nginx está funcionando
      service: name=nginx state=started enabled=yes

    - name: Comprobar que gunicorn está funcionando
      service: name=gunicorn state=started enabled=yes
    

  handlers: 
    - name: restart nginx
      service: name=nginx state=restarted

    - name: restart gunicorn
      service: name=gunicorn state=restarted
```

**gunicorn.j1** Fichero de configuración de gunicorn que será copiado a la VM

```yml
description "Gunicorn application server handling {{ project_name }}"

start on runlevel [2345]
stop on runlevel [!2345]

respawn
setuid www-data
setgid www-data

chdir {{ install_root }}/{{ project_name }}

env PYTHONPATH={{ install_root }}/{{ project_name }}
env ON_HEROKU={{ ON_HEROKU }}
env SECRET_KEY={{ SECRET_KEY }}
env MONGO_URI={{ MONGO_URI }}
env API_KEY={{ API_KEY }}
env DATABASE_URL={{ DATABASE_URL }}

exec /usr/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 {{ wsgi_module }}:application
```

**nginx.j2** Fichero de configuración de nginx que será copiado a la VM

```yml
server {
	listen 80;
	server_name {{ server_name }};
	charset utf-8;


	location /static {
		alias {{ static_root }};
	}

	location / {
		proxy_pass http://localhost:8000;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	}
}
```

Para desplegar la aplicación primero abrimos el puerto 80 de la MV:

``$ azure vm endpoint create musiv1617 80 80``

Luego, ejecutamos ansible (Ya se ha configurado SSH en el ejercicio 1 para permitir la conexión sin clave)

``$ ansible-playbook -i hosts provision.yml``

![Imagen 6-6](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema6-6.png)


##Ejercicio 4
**Instalar una máquina virtual Debian usando Vagrant y conectar con ella.**

He instalado Ubuntu 16 Server (Xenial Xerus)	

Que se puede encontrar [aquí](https://atlas.hashicorp.com/gbarbieru/boxes/xenial)

Descargamos la imagen:

``$ vagrant box add gbarbieru/xenial https://atlas.hashicorp.com/gbarbieru/boxes/xenial``


Iniciamos la imagen:

``$ vagrant init gbarbieru/xenial``


Arrancamos la má:

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

``$ vagrant provision``

##Ejercicio 6
**Configurar tu máquina virtual usando vagrant con el provisionador chef.**

Vamos a instalar nginx pero ahora usando el provisionador Chef:

Creamos una receta para nginx:

```
$ mkdir cookbooks
$ knife cookbook create nginx -o cookbooks
```

Editamos el fichero Edit recipes/default.rb:

```ruby
execute 'apt-get update' do
  action :run
end

package 'nginx' do
  action :install
end
```

Creamos un nuevo fichero Vagrantfile con este contenido:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "gbarbieru/xenial"

  config.vm.provision :chef_solo do |chef|
    chef.run_list = [
      "recipe[nginx]"
    ]
  end

end
```

Ejecutamos:

``$ vagrant provision``

![Imagen 6-7](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema6-7.png)

Conectamos a la máquina para comprobar que se ha instalado nginx:

``$ vagrant ssh``

``$ nginx -v``

![Imagen 6-8](http://i1210.photobucket.com/albums/cc420/mj4ever001/tema6-8.png)


