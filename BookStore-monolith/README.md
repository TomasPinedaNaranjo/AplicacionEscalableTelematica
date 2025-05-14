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

Objetivo 2:

<img width="877" alt="Captura de pantalla 2025-05-13 a la(s) 7 37 50 p m" src="https://github.com/user-attachments/assets/b38a8b02-618d-4391-adc6-067ec300262f" />

Objetivo 3:

<img width="433" alt="Captura de pantalla 2025-05-13 a la(s) 7 38 46 p m" src="https://github.com/user-attachments/assets/6b61eb0a-f289-4c6c-bcf6-acb70245b09d" />

# 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

## como se compila y ejecuta.

## detalles del desarrollo.

## detalles técnicos

## descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)

## opcional - detalles de la organización del código por carpetas o descripción de algún archivo. (ESTRUCTURA DE DIRECTORIOS Y ARCHIVOS IMPORTANTE DEL PROYECTO, comando 'tree' de linux)

##

## opcionalmente - si quiere mostrar resultados o pantallazos

# 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

# IP o nombres de dominio en nube o en la máquina servidor.

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
