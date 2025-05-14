### Proyecto 1 Tópicos E. Telemática

### Asignatura:

## Tópicos especiales en Telemática

### Docente:

## Edwin Montoya Munera

### Por:

## Emanuel Patiño

## Esteban Muriel

## Tomás Pineda

### Escuela de ciencias e ingeniería Universidad EAFIT, sede Medellín 2025-1

# Aplicación Escalable Telematica - Proyecto 2

#

# 1. breve descripción de la actividad

<texto descriptivo>
 1.1.

 Objetivo 1:
  Como primer punto del proyecto en el contexto del desarrollo de aplicaciones escalables, se realizó el despliegue de un proyecto desde GitHub en una instancia de AWS utilizando Docker, Nginx como proxy inverso y certificados SSL representa una arquitectura moderna y eficiente. Docker permite encapsular la aplicación y sus dependencias en contenedores portátiles, facilitando la escalabilidad y el despliegue consistente en distintos entornos. Nginx, como proxy inverso, distribuye el tráfico de manera eficiente entre servicios, mejora el rendimiento y permite gestionar múltiples aplicaciones bajo un mismo dominio. Por su parte, el uso de certificados SSL garantiza la seguridad en la comunicación cifrada entre cliente y servidor, un aspecto fundamental para proteger los datos en redes públicas. Esta combinación de herramientas refleja buenas prácticas en la implementación de servicios robustos, seguros y escalables en la nube.

Objetivo 2:
Tenemos como objetivo el despliegue de una app monolítica en un entorno escalable en AWS. Para esto partimos de las siguientes consideraciones de elementos necesarios para el desarrollo de este objetivo:

Múltiples VMs (EC2) con Auto Scaling Group.
Un Load Balancer (ELB).
Base de datos administrada (RDS) o replicada con HA.
Un sistema de archivos NFS compartido (por ejemplo, EFS en AWS o tu propio NFS en una VM con HA).
El dominio y SSL del objetivo 1 deben seguir funcionando.

Para esto, el objetivo es ejecutar 2 o más VMs con la app y Docker-Compose. Además, la creación de un Auto Scaling Group para que las instancias se creen o se eliminen automáticamente. Necesitaremos un Load Balancer encargado de distribuir el tráfico, una base de datos Amazon RDS con MySQL. Además, aunque no existen archivos en la aplicación, la implementación de un sistema de archivos compartido NFS.

Objetivo 3: Para el tercer objetivo teníamos como propósito utilizar Docker Swarm (o una herramienta similar) como orquestador de contenedores que nos permite el manejo y despliegue de contenedores a través de diferentes nodos, asegurando escalabilidad y alta disponibilidad. Además del uso de una base de datos externa, en este caso RDS de AWS con MySql y un balanceador de cargas para asegurar disponibilidad y detección de instancias/nodos caídos

## 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

# 2. información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.
Objetivo 1:

![Captura de pantalla (124)](https://github.com/user-attachments/assets/e261bd4b-e3e7-435d-8e0c-9f1040e88aba)


Objetivo 2:

<img width="877" alt="Captura de pantalla 2025-05-13 a la(s) 7 37 50 p m" src="https://github.com/user-attachments/assets/b38a8b02-618d-4391-adc6-067ec300262f" />

Objetivo 3:

<img width="433" alt="Captura de pantalla 2025-05-13 a la(s) 7 38 46 p m" src="https://github.com/user-attachments/assets/6b61eb0a-f289-4c6c-bcf6-acb70245b09d" />

# 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

## A continuación se muestra como se compila, ejecuta, detalles del desarrollo, detalles técnicos y descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)
# Para el objetivo 1: 

# Paso 1: Crear la Instancia EC2
Seleccionar Amazon Linux 2023: Elegimos Amazon Linux 2023 como sistema operativo para la instancia EC2.

Permitir tráfico HTTP/HTTPS: Configuramos las reglas de seguridad para permitir el tráfico a través de los puertos HTTP (80) y HTTPS (443), esenciales para la comunicación web.

Asignar IP Elástica: Se asignó una dirección IP estática elástica a la instancia para que sea accesible de forma permanente a través de internet.

IP de la instancia: 13.217.234.178
Configurar DNS con Route 53: Asignamos la IP elástica a la configuración de la zona de Route 53, lo que nos permite usar un dominio personalizado para acceder al servidor, por ejemplo, elsapofeliz.website.

# Paso 2: Instalar Docker y GitHub

Nota: Desplegar aplicaciones en Docker es crucial especialmente para sistemas escalables, ya que permite encapsular servicios en contenedores ligeros, portables y consistentes. Esto facilita la implementación en diferentes entornos sin conflictos de dependencias, optimiza el uso de recursos y mejora la escalabilidad horizontal al permitir replicar servicios rápidamente. Además, Docker simplifica la automatización del despliegue, mantenimiento y monitoreo, lo cual es vital en aplicaciones telemáticas que requieren alta disponibilidad, baja latencia y facilidad para adaptarse a demandas variables.
Instalar Docker:
 Se creó un archivo de script docker.sh para automatizar la instalación de Docker en la instancia. Esto incluye la adición de la clave GPG oficial de Docker, el repositorio, y la instalación de los paquetes necesarios.
Ingresamos con: nano docker.sh

El archivo docker.sh contiene los siguientes comandos:

Poner dentro de un archivo:


# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

Ejecutar el script para instalar Docker y Docker Compose:

chmod +x docker.sh
./docker.sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose

Para no tener que usar sudo siempre con docker hacemos: 

sudo usermod -aG docker $USER
newgrp docker

Configuramos Docker para que inicie automáticamente al arrancar el sistema:
sudo systemctl enable docker.service
sudo systemctl enable containerd.service

Instalamos git: 

sudo apt install -y git
git clone repository
docker-compose up --build -d
docker ps

Ya deberia de poder acceder http://<TU_IP_ELÁSTICA>:5000
NOTA:  puerto 5000 esté abierto en el security group de tu instancia EC2.

# Paso 3: Implementar Nginx como proxy inverso

Nota: Un proxy inverso como Nginx es fundamental en aplicaciones escalables porque actúa como intermediario entre los clientes y los servidores, distribuyendo el tráfico de manera eficiente y segura. Su uso permite balancear la carga entre múltiples instancias de una aplicación, optimizar el rendimiento mediante caché, manejar certificados SSL/TLS para conexiones seguras y redirigir peticiones de forma inteligente. Esto no solo mejora la disponibilidad y la velocidad de respuesta, sino que también proporciona una capa adicional de seguridad y control, aspectos clave en entornos donde la estabilidad y la eficiencia del sistema son esenciales. 

Instalar Nginx:
Se instaló Nginx para actuar como proxy inverso, redirigiendo las solicitudes del usuario a la aplicación en ejecución en el puerto 5000. Los comandos para instalar Nginx son:
sudo apt install nginx -y
sudo service nginx status

Configurar el archivo de Nginx:
Se creó un archivo de configuración para Nginx en /etc/nginx/sites-available/elsapofeliz.website con el siguiente contenido:

nano /etc/nginx/sites-available/elsapofeliz.website

Contiene: 

server {
        listen 80;
        server_name elsapofeliz.website;

        location / {
                proxy_pass http://localhoost:5000;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }
}

Habilitar el sitio:
Se habilita el archivo de configuración creando un enlace simbólico en el directorio sites-enabled:
sudo ln -s /etc/nginx/sites-available/elsapofeliz.website /etc/nginx/sites-enabled/

sudo nginx -t

sudo systemctl restart nginx

# Paso 4: Instalar certificado SSL 

Nota: Implementar SSL (Secure Sockets Layer) es esencial en aplicaciones porque garantiza la confidencialidad e integridad de los datos transmitidos entre los clientes y el servidor. Al cifrar la comunicación, SSL protege la información sensible como credenciales o datos personales contra ataques de intermediarios o interceptaciones maliciosas. En entornos escalables, donde múltiples servicios pueden comunicarse a través de redes públicas o privadas, contar con certificados SSL también refuerza la confianza del usuario y cumple con estándares de seguridad modernos, siendo especialmente relevante cuando se manejan datos en tiempo real o comunicaciones críticas.

sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d elsapofeliz.website
sudo systemctl restart nginx


Este certificado expira para ello verificamos renovación automática:

 sudo systemctl status certbot.timer

## opcional - detalles de la organización del código por carpetas o descripción de algún archivo. (ESTRUCTURA DE DIRECTORIOS Y ARCHIVOS IMPORTANTE DEL PROYECTO, comando 'tree' de linux)

##

## opcionalmente - si quiere mostrar resultados o pantallazos

# 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

# IP o nombres de dominio en nube o en la máquina servidor.

# Dominio: elsapofeliz.website

## descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)

## como se lanza el servidor.

## una mini guia de como un usuario utilizaría el software o la aplicación

## opcionalmente - si quiere mostrar resultados o pantallazos

# 5. otra información que considere relevante para esta actividad.

# referencias:

<debemos siempre reconocer los créditos de partes del código que reutilizaremos, así como referencias a youtube, o referencias bibliográficas utilizadas para desarrollar el proyecto o la actividad>

## sitio1-url

## sitio2-url

## url de donde tomo info para desarrollar este proyecto

https://www.youtube.com/watch?v=bEI8ScZWYjE&list=LL&index=1&t=571s

https://wordspiner.xyz/how-to-install-docker-on-kali-linux-with-a-few-simple-steps/

