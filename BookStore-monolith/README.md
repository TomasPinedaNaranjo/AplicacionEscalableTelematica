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

Objetivos cumplidos:
RF1.1: La aplicación BookStore debe ser desplegada correctamente en una instancia EC2 de AWS usando Docker.

RF1.2: El sistema debe estar accesible desde un dominio personalizado configurado correctamente.

RF1.3: NGINX debe actuar como proxy inverso y gestionar las conexiones HTTPS mediante un certificado SSL válido.

RF2.1: El sistema debe escalar automáticamente mediante un grupo de autoescalamiento de EC2 al detectar carga alta.

RF2.2: La base de datos debe estar separada del backend de la aplicación y debe ser gestionada mediante un servicio administrado (por ejemplo, Amazon RDS) o configurada con alta disponibilidad.

No cumplidos:
Dado a que se transformó la idea inicial del objetivo 3 del proyecto no logramos realizar estos requisitos puntuales ya que se transformó por una implementación de Docker Swarm.

RF2.3: Los servidores deben tener acceso a archivos compartidos a través de un sistema de archivos NFS disponible y tolerante a fallos.

RF3.1: El microservicio de Autenticación debe permitir a los usuarios registrarse, iniciar sesión y cerrar sesión.

RF3.2: El microservicio de Catálogo debe permitir la consulta de los libros disponibles en la plataforma.

RF3.3: El microservicio de Compra debe permitir realizar el proceso completo de compra, incluyendo pago y confirmación de entrega.

Requisitos funcionales cumplidos:

RNF: El sistema debe permitir el escalamiento horizontal automático.

RNF: La plataforma debe mantener un tiempo de disponibilidad.

RF: Debe haber distribución de tráfico. 

Requisitos No funcionales cumplidos:

Estos requisitos relacionados con el objetivo 3 no se lograron ya que este objetivo se transformó.

RNF: Baja latencia entre la comunicación entre microservicios.

RNF: Escalabilidad individual entre cada microservicio.

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


# Paso 5: Verificar el funcionamiento de todo el sistema:

Docker:

![Captura de pantalla (150)](https://github.com/user-attachments/assets/247ea984-fb84-42da-bc35-5cd5846b9fa4)

Nginx:

![Captura de pantalla (151)](https://github.com/user-attachments/assets/a7686899-bc19-4336-a83c-694e6ad13a42)

SSL:

![Captura de pantalla (152)](https://github.com/user-attachments/assets/5c5a2735-ae7c-483a-9756-315ce2d9dae1)

# Desarrollo del objetivo 2
Tenemos como objetivo el despliegue de una app monolítica en un entorno escalable en AWS. Para esto partimos de las siguientes consideraciones de elementos necesarios para el desarrollo de este objetivo:

Múltiples VMs (EC2) con Auto Scaling Group.
Un Load Balancer (ELB).
Base de datos administrada (RDS) o replicada con HA.
Un sistema de archivos NFS compartido (por ejemplo, EFS en AWS o tu propio NFS en una VM con HA).
El dominio y SSL del objetivo 1 deben seguir funcionando.

Para esto, el objetivo es ejecutar 2 o más VMs con la app y Docker-Compose. Además, la creación de un Auto Scaling Group para que las instancias se creen o se eliminen automáticamente. Necesitaremos un Load Balancer encargado de distribuir el tráfico, una base de datos Amazon RDS con MySQL. Además, aunque no existen archivos en la aplicación, la implementación de un sistema de archivos compartido NFS.

Se definieron el siguiente conjunto de pasos para cumplir con dicho objetivo.
Crear una imagen AMI de la instancia del objetivo 1
Configurar RDS (MySQL)
Montar EFS
Configurar el Auto Scaling Group
Crear el ALB

Decidimos utilizar el patrón de arquitectura de escalamiento de app monolítica Escalamiento horizontal con ALB + Auto Scaling + almacenamiento externo. A continuación se describe cada uno de los componentes claves del patrón:

Balanceador de carga (ALB)

Distribuye tráfico entrante a múltiples instancias.
Soporta SSL y redirección automática.

Auto Scaling Group

Crea más instancias EC2 cuando sube la carga (escalamiento horizontal).
Elimina instancias cuando baja la carga (optimización de costo).
Usa una AMI con Docker + App ya instalada.

Base de datos externa (Amazon RDS)

Las instancias NO deben tener su propia DB local.
Todas comparten una sola base de datos escalable y confiable.

Sistema de archivos compartido (Amazon EFS)

Si la app guarda archivos (por ejemplo, imágenes subidas por usuarios), debe usar un almacenamiento común.
Todas las instancias montan el mismo volumen EFS.

# 1. Creación de base de datos RDS
Vamos a RDS y seleccionamos crear base de datos MySQL, entre las configuraciones seleccionamos el VPC default, además del free tier y la versión MySQL 8.0.40. Es importante seleccionar un nombre de administrador y contraseña segura para conectarse a la DB desde las instancias.

![Captura de pantalla (126)](https://github.com/user-attachments/assets/3fde1c9b-a8f4-45b9-b7f5-3e52932c8de9)

Como siguiente paso para conectar la base de datos creada a las instancias corriendo nuestra aplicación flask, debemos modificar el archivo docker compose por el cual se crea y corre nuestra app. El archivo por defecto está configurado para levantar dos contenedores, uno para la aplicación y otro para la base de datos. Ahora la nueva versión de este docker compose solo levanta un contenedor para la aplicación y se conecta a la base de datos externa. El DB_HOST en este caso es el endpoint que obtenemos de la base de datos RDS.

![Captura de pantalla (127)](https://github.com/user-attachments/assets/2bbc1dd3-08fe-43e4-a622-8f71bf1bf63f)

Como se puede observar, los datos sensibles para la conexión a la base de datos se obtienen de un archivo .env creado con el objetivo de proteger dicha información.

El archivo config.py tambien sufre cambios necesarios para la conexión a la base de datos:

![Captura de pantalla (128)](https://github.com/user-attachments/assets/182c13c7-0d63-4015-b2a4-f9b912aebe94)

En el archivo app.py realizamos la importacion del archivo configurado anteriormente config.py 

![Captura de pantalla (129)](https://github.com/user-attachments/assets/ff04b9b8-2284-45b1-b1b1-83243b88e77f)

Ahora debemos ir a la instancia en donde tenemos nuestra aplicación del objetivo 1 y nos podemos acceder al shell sql con el comando “mysql -h bookstoredb.c7hyjtnhxg7c.us-east-1.rds.amazonaws.com -u admin -p”.
Es importante también levantar nuevamente el contenedor con los nuevos cambios del Docker-Compose.

Vemos que después de crearse la base de datos podemos observar y todas las instancias que tengan la aplicación ejecutando se han conectado a ella de forma remota gracias a las configuración y modificaciones anteriores.

![Captura de pantalla (130)](https://github.com/user-attachments/assets/0c8a68e1-c37a-4752-bfc5-399ec0d626b4)

# 2. Creación de EFS.
Ahora crearemos el sistema de archivos compartido NFS. Para esto nos dirigimos a Amazon EFS y creamos un nuevo EFS. En esta parte solo deberemos seleccionar un nombre y elegir la VPC default.
Ahora deberemos copiar la identificación de este EFS creado la cual será necesaria para montarlo en la instancia donde tenemos nuestra app. 

![Captura de pantalla (131)](https://github.com/user-attachments/assets/f56c4132-93fa-4aed-a6f7-a77798192f42)

Ahora ejecutamos este comando en la instancia para montar el EFS creado, junto con la carpeta donde estarán los archivos compartidos por las diferentes instancias.

sudo mount -t nfs4 -o nfsvers=4.1 fs-05215bfd3a2439b87.efs.us-east-1.amazonaws.com:/ /mnt/efs
Podemos comprobar que el EFS quedó correctamente montado en la instancia a continuación:

![Captura de pantalla (132)](https://github.com/user-attachments/assets/4763aeea-d3dd-4933-9e26-a8733c700ea4)

Ahora es importante definir en nuestro Docker-Compose el volumen donde podrá subir sus archivos.

![Captura de pantalla (133)](https://github.com/user-attachments/assets/029f67af-0c3c-4969-abcc-92d60c77f81b)

Una vez que tenemos nuestra instancia con el Docker-compose de forma adecuada con su conexión a la base de datos y el sistema EFS montado, procedemos a crear la imagen AMI necesaria para los pasos posteriores.

![Captura de pantalla (134)](https://github.com/user-attachments/assets/95eb7187-6a13-4d3d-948c-1fed07d15fba)

# 3. Creación de imagen AMI
Para la creación de la imagen AMI vamos a tomar la instancia base donde ya tenemos todo lo descrito anteriormente y vamos a crear Imagen, a continuación observamos la imagen creada.

![Uploading Captura de pantalla (134).png…]()

# 4. Creación de Launch Template

Ahora vamos a crear Launch Template y creamos una nueva. Esta template podemos crearla usando un user-data o una imagen AMI como base, seleccionaremos la imagen AMI creada anteriormente la cual ya tiene una copia de todo aquello que tenemos en la instancia base que hemos configurado desde el inicio. Al crear la Launch template se selecciona la VPC default con los grupos de seguridad por defecto.

Observamos la Launch Template creada y su configuración:

![Captura de pantalla (135)](https://github.com/user-attachments/assets/9be86c92-fc10-4c27-b197-0cc6b304000e)

# 5. Creación del ASG
Nos dirigimos a ASG en amazon y creamos un Auto Scaling Group seleccionando la plantilla de lanzamiento creada en el paso anterior. En la configuración de redes y subredes, elegimos la VPC que hemos venido utilizando y seleccionamos al menos dos subredes públicas.
 Debemos configurar los siguientes aspectos: 
Tamaño del grupo
Capacidad deseada: 2
Mínimo: 2
Máximo: 4.

Esto mantendrá siempre al menos 2 instancias activas.

Política de escalado
Política de escalado basada en métricas.
CPU utilization > 70% para escalar hacia arriba.

Y tendríamos nuestro grupo creado

![Captura de pantalla (136)](https://github.com/user-attachments/assets/c62e3d7c-6abd-48bf-b865-593cc1d3ed43)

El siguiente paso es la creación del Load Balancer para distribuir la carga entre los diferentes instancias que se generen en el ASG.

![Captura de pantalla (137)](https://github.com/user-attachments/assets/22eb74a8-d409-4f36-99fc-74edd5c4ee1e)

# 6. Creación del ALB

Vamos ALB en Amazon y creamos un Load Balancer, seleccionamos el tipo ALB y en esquema seleccionamos Internet-facing. Al igual que los demás componentes seleccionamos el VPC por default y seleccionamos al menos dos redes públicas. Tipo de IPv4 y establecemos el listener en el puerto 80 (HTTP).
Es importante crear un target group que tendrá el conjunto de instancias (en este caso el ASG ya creado) entre las cuales el ALB balanceara las peticiones. Cuando el ASG crea nuevas instancias, el se encarga automáticamente de añadirlas a este target group del ALB.

A continuación se ve el ALB creado y sus configuraciones.

![Captura de pantalla (138)](https://github.com/user-attachments/assets/2e9bcce1-8258-49da-9ca1-fab1fbb67dcd)

![Captura de pantalla (139)](https://github.com/user-attachments/assets/69644b5c-ddab-4861-9e9f-5bff1b6b4e7d)

Ahora asociar el ALB con el ASG como se ve a continuación:

En la sección de "Load balancing":
Seleccionamos "Attach to an existing load balancer" y aquí elegimos nuestro ALB junto con el Target Group que hemos creado

![Captura de pantalla (140)](https://github.com/user-attachments/assets/297df66f-77c6-450d-aca4-65153eab7fef)

![Captura de pantalla (141)](https://github.com/user-attachments/assets/04f770e0-61a0-4783-9052-3a7dc707244a)

De esta forma, cuando el ASG lance una nueva instancia, automáticamente se registrará en el Target Group y el ALB empezará a enrutar tráfico a esa instancia.

# Algunas pruebas

Vamos a acceder a la dirección ip del balanceador para ver si nos redirige a la aplicación.

![Captura de pantalla (142)](https://github.com/user-attachments/assets/6ef839be-317e-473c-b655-80ee6cbd7338)

En este momento se están ejecutando ambas instancias generadas por el ASG para mantener el estado deseado de acuerdo a lo que configuramos. Si la página aumenta demasiado su tráfico y número de peticiones, el ASG creará nuevas instancias hasta un máx de 4 para manejar y distribuir esta carga.
Al tratarse de una aplicación bastante que no recibe muchas peticiones no es posible por el momento llevarla al extremo para lograr cambiarla.

Ahora crearemos un libro para comprobar que se ejecuta adecuadamente la base de datos compartida gestionada por RDS.

Accederemos desde una máquina y crearemos un libro.

![Captura de pantalla (143)](https://github.com/user-attachments/assets/0ed9291e-fc17-4334-8ea0-d1668bb7043d)

Accedemos desde otra máquina y vemos el libro creado:

![Captura de pantalla (144)](https://github.com/user-attachments/assets/3362fc37-c7c3-4454-ad42-4a9a6e319586)

Ahora, si tumbamos una de las instancias, vemos que el load balancer y el ASG no permiten que la app caiga y esta simplemente cambia de instancia y funciona normalmente.

![Captura de pantalla (145)](https://github.com/user-attachments/assets/847e51a4-49bd-4e59-b00f-bd1d4e2184e3)

Vemos desde consola los libros creados:

![Captura de pantalla (146)](https://github.com/user-attachments/assets/9f805986-211c-44bc-b51d-cf30314082a5)

Para finalizar observamos el grupo de seguridad y las reglas de entrada establecidas para el logro de este objetivo:

![Captura de pantalla (147)](https://github.com/user-attachments/assets/11a1ee47-9985-4a88-b5f6-4760adc3c955)

A modo de conclusión:
En esta fase, logramos escalar horizontalmente nuestra aplicación monolítica Bookstore en AWS. Configuramos Amazon RDS como base de datos externa y Amazon EFS como almacenamiento compartido para archivos. Adaptamos la aplicación y docker-compose para funcionar con estos servicios externos.

Creamos una AMI personalizada, un Launch Template y un Auto Scaling Group que mantiene múltiples instancias activas y escalables. Finalmente, integramos un Application Load Balancer (ALB) que distribuye el tráfico a las instancias del grupo, asegurando alta disponibilidad y tolerancia a fallos.


# Configuración Objetivo 3 (Docker Swarm)
Para el tercer objetivo teníamos como propósito utilizar Docker Swarm (o una herramienta similar) como orquestador de contenedores que nos permite el manejo y despliegue de contenedores a través de diferentes nodos, asegurando escalabilidad y alta disponibilidad. Además del uso de una base de datos externa, en este caso RDS de AWS con MySql y un balanceador de cargas para asegurar disponibilidad y detección de instancias/nodos caídos


Lo primero es crear una Launch template y la configuración avanzada poner los siguientes comandos para instalar lo necesario para el correcto funcionamiento de la app:
#!/bin/bash
apt update -y
apt install -y docker.io
systemctl start docker
systemctl enable docker
apt install -y curl python3-pip
pip3 install docker-compose
docker-compose --version


Luego, creamos 4 instancias a partir de esta nueva plantilla y esperamos que se inicialicen correctamente.
Debemos asegurarnos de entrar a los securty groups y habilitar los siguientes puertos:

![Captura de pantalla (148)](https://github.com/user-attachments/assets/17697e96-cb49-4a14-8164-9f61a4221c90)

Ingresamos a la 4 máquinas virtuales y escogemos una que va a ser el líder.
Este nodo líder efectuamos el comando para habilitar docker swarm: 
sudo docker swarm init
Luego de ejecutar este comando en la consola se mostrara un comando que deberemos copiar y pegar en las otras 3 máquinas para que se unan como managers al cluster (algo similar a esto):
sudo docker swarm join --token <token> <IP_VM_1>:2377
Si ejecutamos el siguiente comando deberíamos ver los 4 nodos:
sudo docker node ls
Luego creamos/configuramos la red Overlay en docker swarm con el siguiente comando (desde el nodo 1 o el líder)
sudo docker network create --driver=overlay bookstore_net
Luego creamos un archivo docker-compose.yml con la siguiente información o nos lo descargamos desde el repositorio https://github.com/TomasPinedaNaranjo/AplicacionEscalableTelematica.git en la rama llamada obj3

![Captura de pantalla (149)](https://github.com/user-attachments/assets/5d8c0cf2-18cb-445e-a7b9-045f193143e6)

(había otra manera de configurar las variables ENV, pero no se lo logró que funcionara, entonces tocó así)
Luego desplegamos la aplicación usando Docker Stack con el siguiente comando:
sudo docker stack deploy -c docker-compose.yml bookstore
Podemos verificar el estado de los servicios y ver detalles de las réplicas con:
sudo docker service ls
	sudo docker service ps bookstore_flaskapp

Además, en cada máquina podemos ver los nodos/replicas (que en total son 10) que se le han asignado a cada MV usando el comando:
	
sudo docker node ls

Por último, AWS creamos un Load Balancer que escuche a las 4 máquinas virtuales creadas anteriormente en el puerto 5000 y accedemos al DNS name para ingresar a la página.

Docker swarm se encargará de redistribuir las réplicas en caso de que alguna máquina virtual se caiga, además de también distribuir cargas por sí mismo (el load balancer es más que todo para no tener que acceder a la app por medio de las IPs públicas de las instancias y no tener que preocuparse por ver si alguna instancia esta caída)

## opcional - detalles de la organización del código por carpetas o descripción de algún archivo. (ESTRUCTURA DE DIRECTORIOS Y ARCHIVOS IMPORTANTE DEL PROYECTO, comando 'tree' de linux)

##
# IP o nombres de dominio en nube o en la máquina servidor.

# Dominio: elsapofeliz.website

## sitio1-url

## sitio2-url

# referencias:

## url de donde tomo info para desarrollar este proyecto

https://www.youtube.com/watch?v=bEI8ScZWYjE&list=LL&index=1&t=571s

https://wordspiner.xyz/how-to-install-docker-on-kali-linux-with-a-few-simple-steps/

