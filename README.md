# Practica de balanceo de cargas de alta disponibilidad 

La práctica implica crear cinco (bastion, servidor 1, servidor 2, servidor 3 y servidor frontal) utilizando contenedores con imágenes de Ubuntu y NGINX.

El servidor frontal recibe las peticiones de los clientes, balancea las peticiones a los 3 servidores de la aplicación web, en este caso servidor 1, servidor 2 y servidor 3, los cuales contienen dentro otro balanceador de cargas Nginx y 3 instancias de la aplicación web
El servidor de bastion es el encargado de hacer las configuraciones mediante Ansible, exponendo el puerto 22 con el 22 del equipo local

## Como funciona

Primero se configura el archivo de **docker-compose.yml** en donde se definen los 5 servidores
se configura el puerto 22 y se cambia la contraseña de root

### Bastion

El primer servidor es el bastion el cual configure dejando el puerto 22 abierto y cambiando el password de root para poder acceder desde cualquier computadora.

En el archivo de **DockerfileBastion** se configuran los paquetes necesarios para el funcionamiento
 - acutalización *update*
 - openssh-server
 - ansible
 - python3
 - nano

### Web Apps

Lo siguiente fue la configuración de los sevidores server-1, server-2 y server-3, activar los privilegios para que pueda correr el docker daemon esto en **(DockerfileServer)**
Se instalan los paquetes y las actualizaciones necesarias así como la activación de servicios de ssh
 - acutalización *update*
 - openssh-server
 - sudo
 - python3
 - nano
 - git

Git nos servirá para clonar el repositorio con la aplicación web desde:
https://gitlab.com/Juan-Angel-Cepeda/balanceo-cargas

### Servidor de Nginx

Se encuentra en **(nginx.conf)**

## Configuracion mediante ansible

Nos conectambos a bastion con ssh para hacer las configuraciones
hacemos la configuración de hosts de ansible que se encuetran en el archivo **(hosts)**

Nos conectamos a los servidores para agregar la llave pubica de bastion a cada web server

Ejecutando el archivo de **(playbookSecurity.yml)** el cual desactiva las conexiones de ssh de los servidores web

Si todo esta bien configurado ya se puede ejecutar el docker-compose para poner en funcionamiento la arquitectura, esto en el archivo **(playbook.yml)** el cual instalará curl, docker, clonará el repo de git y hará el docker compose