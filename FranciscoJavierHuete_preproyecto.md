<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fira+Sans:wght@400;700;800;900&display=swap" rel="stylesheet">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fira+Code&display=swap" rel="stylesheet">

# Pre - Proyecto

Francisco Javier Huete Mejías

## Índice

- [Título del proyecto](#título-del-proyecto)
- [Descripción del proyecto](#descripción-del-proyecto)
- [Tecnologías que se van a usar](#tecnologías-que-se-van-a-usar)
- [Resultados esperados](#resultados-esperados)

<div class="page"/>

# Título del proyecto

Despliegue de la herramienta de gestión de la infraestructura de red NetBox usando Caddy como proxy inverso.

<div class="page"/>

# Descripción del proyecto

NetBox es una aplicación web escrita en Python usando el framework Django que permite gestionar y administrar la infraestructura de una red local. Esta aplicación usa una base de datos PostgresSQL para almacenar la información y Redis para la gestión de tareas. Además, también requiere de un servicio WSGI como gunicorn o uWSGI y un servidor web como Nginx o Caddy para servir la aplicación.

Esta aplicación ofrece una solución que permite modelar y documentar redes actuales. Combina las disciplinas tradicionales de la gestión de direcciones IP (IPAM o IP address management) con la de gestión de la infraestructura del centro de datos (DCIM o datacenter infrastructure management). Además cuenta con importantes APIs y extensiones.

Por su parte, Caddy es un servidor web escrito en Go que aporta funcionalidades nuevas que no son habituales en otros servidores web tradicionales. Es un proyecto de la empresa ZeroSSL, que trabaja en el ámbito de la certificación de dominios. Esto hace que una de las características principales de Caddy sea que, por defecto, sirve todo el contenido de manera segura usando TLS, incluso para localhost o para las conexiones desde IP internas. Además, la configuración de este servidor web se puede hacer usando un fichero JSON o a través de una API RESTful.

El objetivo de este proyecto es implantar un servidor capaz de ofrecer en una red local NetBox. Esta aplicación de gestión de la red fue desarrollada originalmente por el equipo de DigitalOcean en 2015 y se liberó como un proyecto de código abierto en junio de 2016. NetBox pretende que los administradores de la red puedan documentar y gestionar todos los recursos disponibles, tanto las direcciones IP, las diferentes VLAN, los dispositivos físicos que forman parte de la red o las conexiones cableadas entre estos dispositivos.

Esta aplicación se ejecuta como un servicio WSGI detrás de un servidor web. Además, necesita funcionar junto a un servicio de Postgres y uno de Redis. En su documentación oficial se recoge la información necesaria para implantar esta herramienta tanto instalando todo el software necesario en el sistema operativo como de una forma más aislada del sistema, usando contenedores Docker.

En cualquiera de los dos casos, para permitir el acceso a la aplicación, es necesario configurar un proxy inverso usando un servidor web. En este caso, el servidor web elegido es Caddy. Con esta elección se persigue también como objetivo en el proyecto demostrar el proceso de instalación de este servidor web en una máquina, así como los diferentes métodos de configuración que esta herramienta permite.

Adicionalmente, Caddy cuenta con la particularidad de que incluye una herramienta para configurar, de manera sencilla, el protocolo HTTPS en el servidor web para un dominio determinado. Este es otro de los aspectos relevantes a tratar dentro del proyecto.

<div class="page"/>

# Tecnologías que se van a usar

Esta aplicación se puede desplegar tanto instalando los diferentes servicios en el sistema operativo como [usando contenedores Docker](https://github.com/netbox-community/netbox-docker/tree/release). En este caso, el proyecto se puede desarrollar en una o varias máquinas virtuales, por ejemplo, en OpenStack. En cualquier caso, la máquina que sirva la aplicación a través del proxy inverso configurado en el servidor web debe tener una IP flotante que permita su acceso desde el exterior. Así, los diferentes servicios necesarios para el funcionamiento de la aplicación se pueden instalar en una única máquina, en un despliegue monolítico, o en varias instancias, en un despliegue multiservicio en el que cada servicio se ejecuta en una máquina diferente. 

Como posible ampliación del contenido del proyecto, se podría realizar una versión paralela de la instalación usando contenedores Docker. En este caso, todos los contenedores se ejecutan en la misma instancia pero cada servicio se ejecuta en un contenedor aislado.

Para implantar este proyecto en la máquina se deben ejecutar los servicios mencionados anteriormente: la propia aplicación con Django, la base de datos Postgre, la base de datos Redis, el servidor de aplicaciones python (por ejemplo uWSGI o gunicorn) y el servidor web Caddy.

De esta manera, el proceso para implementar la herramienta que es objeto de estudio en este proyecto constará de cuatro fases clave. En la primera de ellas, se preparará el entorno con la creación de las diferentes instancias de OpenStack que alojarán los servicios necesarios para el funcionamiento de la aplicación. 

La segunda fase supondrá la configuración básica del escenario a través de la instalación de las dependencias para NetBox como Python, Postgres, Redis, el servidor de aplicaciones, etc. En esta fase puede ser interesante el uso de tecnologías propias de la filosofía DevOps como, por ejemplo, Ansible para automatizar la configuración de las diferentes instancias en un playbook.

A continuación, en una tercera fase, se realizará el despliegue de la aplicación clonando el repositorio publicado en GitHub, configurando los ficheros de necesarios y ejecutando los comandos propios para la implantación de una aplicación Django como `migrate`, `createsuperuser` o `collectstatics`. 

La última fase tendrá como objetivo hacer accesible la aplicación desde el exterior. Para ello, se usará el servidor web Caddy como proxy inverso para redirigir al servidor de aplicaciones. Además, se configurará también en este servidor web la redirección a HTTPS.

<div class="page"/>

# Resultados esperados

Al concluir el proyecto, se espera conseguir una o varias instancias en OpenStack que cuenten con los servicios necesarios para que la herramienta sea funcional. En cualquier caso, al terminar el proyecto se espera contar con una máquina ofrece a los clientes de la red local una aplicación de gestión de la red.

El contenido de la memoria se centrará en detallar los aspectos fundamentales de los pasos que se deben seguir para la instalación de cada uno de los servicios necesarios para que la aplicación se ejecute adecuadamente. Esta guía de instalación tendrá en cuenta las particularidades de las diferentes herramientas que se usarán en la pila LAMP como, por ejemplo, las relacionadas con el uso del servidor web Caddy. Además, también incluirá una guía de uso de la propia herramienta para la gestión de una red local.

En la presentación del proyecto se pueden mostrar, de manera resumida, los pasos necesarios para la instalación de la aplicación y, de forma algo más detallada, los diferentes métodos de configuración del proxy inverso usando el servidor web Caddy, así como los principales apartados de la interfaz de esta herramienta de gestión de redes con algún ejemplo de uso.