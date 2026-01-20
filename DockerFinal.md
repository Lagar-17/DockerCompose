# 💻 DOCKER COMPOSE - PROYECTO FINAL 

## 📄 Preguntas

* 🔌 ***¿Qué son los contenedores de docker?***

Los contenedores de Docker son unidades de software ligeras, portables y autosuficientes que empaquetan una aplicación junto con todas sus dependencias (librerías, configuraciones, archivos de sistema) necesarias para ejecutarse.

 * Ejemplo práctico - Ejecutar un servidor web Nginx en un contenedor: docker run -d -p 80:80 nginx
    * El contenedor incluye: Nginx instalado, todas las librerías necesarias, configuración por defecto, sistema de archivos mínimo

**Características principales:**
  * Aislamiento:
    * Cada contenedor se ejecuta en su propio entorno aislado
    * Tiene su propio sistema de archivos, procesos, red y usuarios
    * No puede interferir con otros contenedores ni con el sistema host
  * Ligereza:
    * Comparten el kernel del sistema operativo host
    * No necesitan un sistema operativo completo como las máquinas virtuales
    * Arrancan en segundos (vs minutos de una VM)
    * Consumen menos recursos (CPU, RAM, disco)
  * Portabilidad:
    * "Funciona en mi máquina" = "Funciona en todas las máquinas"
    * Se pueden ejecutar en cualquier sistema con Docker instalado
    * Mismo comportamiento en desarrollo, testing y producción
  * Inmutabilidad:
    * Se crean a partir de imágenes de solo lectura
    * Los cambios se pueden guardar en nuevas capas
    * Fácil de versionar y revertir

* 🔌 ***¿Qué diferencias hay entre los contenedores de docker y los lxc?***

**Docker:**

* Docker está orientado a la ejecución de una sola aplicación por contenedor
* Es muy ligero, rápido de iniciar y altamente portable, ya que funciona prácticamente igual en cualquier sistema que tenga Docker instalado
* Su ecosistema es amplio y maduro, con Docker Hub ofreciendo millones de imágenes listas para usar
* La gestión de red, almacenamiento y recursos está abstraída y simplificada, lo que lo hace ideal para microservicios, CI/CD, desarrollo y despliegue rápido de aplicaciones, así como para su integración con orquestadores como Kubernetes

**LXC:**

* LXC (Linux Containers), en cambio, se enfoca en la virtualización a nivel de sistema operativo, ofreciendo contenedores que se comportan como sistemas Linux completos
* Cada contenedor puede incluir systemd, múltiples servicios y usuarios, de forma similar a una máquina virtual ligera, aunque compartiendo el kernel del host
* Esto lo hace más pesado y algo más complejo de configurar, pero muy útil cuando se necesitan entornos completos, aislamiento de servicios legacy o una alternativa más liviana a las máquinas virtuales tradicionales

En términos de arquitectura, Docker utiliza imágenes en capas de solo lectura y un sistema de archivos por unión (como OverlayFS), mientras que LXC trabaja con un sistema de archivos completo por contenedor. La gestión de recursos en Docker suele hacerse de forma declarativa (por ejemplo, con Docker Compose), mientras que en LXC se configura directamente por contenedor.

* 🔌 ***¿Cuál es la diferencia entre una imagen y un contenedor en docker?***

**Imagen:**

* Es una **plantilla de solo lectura**
* Contiene el código, librerías, dependencias y configuración
* Es **inmutable** (no cambia)
* Se puede compartir y versionar
* Es como una "clase" en programación orientada a objetos
* Comandos útiles:
   * docker images - (listar imagenes locales)
   * docker pull nginx:latest - (Descargar una imagen)
   * docker build -t mi-app:1.0 . - (Construir una imagen desde Dockerfile)
   * docker rmi nginx:latest - (Eliminar una imagen)
   * docker history nginx - (Ver capas de una imagen)

**Contenedor:**

* Es una instancia en ejecución** de una imagen
* Añade una capa de escritura sobre la imagen
* Es temporal
* Tiene estado y puede modificarse
* Es como un "objeto" instanciado de una clase
* Comandos útiles:
   * docker run -d --name web1 nginx - (Crear y ejecutar un contenedor desde una imagen)
   * docker ps - (Listar contenedores en ejecución)
   * docker ps -a - (Listar todos los contenedores (incluso detenidos))
   * docker stop web1 - (Detener un contenedor)
   * docker start web1 - (Iniciar un contenedor detenido)
   * docker rm web1 - (Eliminar un contenedor)

**Ejemplo visual de ambos:**

<img width="508" height="556" alt="image" src="https://github.com/user-attachments/assets/be174c83-4531-4c13-8f1a-136c180c8004" />


* 🔌 ***¿Qué sucede con los datos cuando un contenedor se elimina?***

En Docker, los datos de un contenedor se pierden al eliminarlo, salvo que estén guardados en volúmenes o bind mounts.

**Existen cuatro tipos principales de almacenamiento:**

Capa de escritura del contenedor:
* Es efímera. Los datos desaparecen cuando el contenedor se elimina, no se comparten con otros contenedores y tienen rendimiento limitado. No es adecuada para información importante.

Volúmenes Docker (recomendados):
* Son persistentes y gestionados por Docker. Los datos sobreviven a la eliminación del contenedor, pueden compartirse entre varios contenedores, ofrecen mejor rendimiento y facilitan los backups. Son la opción ideal para datos de producción, como bases de datos.

Bind mounts:
* También son persistentes, ya que enlazan directamente un directorio del host con el contenedor. Permiten acceso directo desde el sistema anfitrión y son muy útiles en desarrollo (por ejemplo, hot-reload), aunque dependen de la estructura del host y son menos portables que los volúmenes.

tmpfs mounts:
* Son efímeros y solo existen en memoria RAM. Los datos se pierden al detener el contenedor, pero ofrecen alta velocidad y mayor seguridad al no escribirse en disco, siendo adecuados para datos temporales o sensibles.
En la práctica, sin volúmenes los datos se pierden (como en una base de datos MySQL sin almacenamiento persistente), mientras que con volúmenes los datos se conservan incluso al recrear el contenedor.

**Buenas prácticas:**
* Usar volúmenes para datos de producción
* Usar bind mounts en desarrollo
* Realizar backups periódicos de los volúmenes
* Evitar guardar datos críticos en la capa de escritura del contenedor

* 🔌 ***¿Cuáles son las ventajas de utilizar contenedores de docker?***

Docker ofrece múltiples ventajas que lo convierten en una tecnología clave para el desarrollo y despliegue moderno de aplicaciones.

* Su principal beneficio es la portabilidad: una aplicación se construye una sola vez y puede ejecutarse de forma idéntica en cualquier entorno (desarrollo, pruebas, producción o cloud), eliminando el clásico problema de “en mi máquina funciona”
* Docker también destaca por su ligereza y eficiencia frente a las máquinas virtuales. Los contenedores ocupan menos espacio, arrancan en segundos, consumen menos memoria y comparten el kernel del sistema operativo, lo que permite ejecutar muchas más aplicaciones en el mismo hardware
* El aislamiento garantiza que cada aplicación tenga sus propias dependencias y versiones sin conflictos, además de mejorar la seguridad mediante mecanismos como namespaces, cgroups y políticas de control de acceso
* Otra ventaja importante es el versionado y control de cambios: las imágenes pueden versionarse fácilmente y permiten realizar rollbacks rápidos si una actualización falla. Esto se complementa con la consistencia entre entornos, ya que la misma configuración (por ejemplo con Docker Compose) se usa en desarrollo, testing y producción, variando solo las variables de entorno
* Docker facilita la escalabilidad horizontal, permitiendo levantar múltiples instancias de una aplicación de forma sencilla y soportando orquestadores como Kubernetes para grandes despliegues. También acelera el desarrollo, ya que permite levantar stacks completos (bases de datos, caches, APIs, frontend) en segundos, sin instalar software adicional en el equipo local
* En cuanto al mantenimiento, las actualizaciones son simples y seguras, con posibilidad de revertir cambios rápidamente. Todo esto contribuye a una reducción de costes, ya que se aprovecha mejor el hardware disponible
* Docker es clave en testing y CI/CD, al permitir ejecutar pruebas en entornos idénticos a producción y automatizar pipelines. Además, encaja perfectamente con arquitecturas de microservicios, donde cada servicio se desarrolla, despliega y escala de forma independiente
* Por último, Docker actúa como documentación ejecutable (a través del Dockerfile) y facilita el debugging y la experimentación, permitiendo probar tecnologías sin instalaciones permanentes.

* 🔌 ***¿Qué tipo de aplicaciones y servicios se pueden desplegar con docker?***

Con Docker se puede ejecutar prácticamente cualquier aplicación.

Docker es válido para casi todos los tipos de software moderno. Se usa ampliamente para aplicaciones web (frontend y backend en Node.js, Python, Java, PHP, Go, .NET, etc.), bases de datos relacionales y NoSQL (MySQL, PostgreSQL, MongoDB, Redis), y herramientas de desarrollo como IDEs web, gestores de bases de datos y sistemas de documentación.

También es clave en CI/CD y DevOps, ejecutando herramientas de automatización, registros de contenedores, monitorización y logging. En áreas más avanzadas, Docker soporta Big Data, Machine Learning e IA, MLOps, así como testing automatizado y control de calidad de código.

Además, se usa para microservicios, servidores de juegos, sistemas de comunicación (chat, correo, videoconferencia), almacenamiento en la nube, seguridad y redes (VPN, proxies, firewalls), CMS, e-commerce, multimedia, búsqueda, IoT, ERP/CRM y backends móviles.

Docker permite incluso levantar stacks completos (frontend, backend, base de datos, caché, colas, monitorización y logs) con un solo archivo de configuración, garantizando consistencia entre entornos.

* 🔌 ***¿Qué otros tipos de contenedores existen además de docker?***

**Aplicaciones Web y Documentación**
* GitLab CE: Gestión de código, CI/CD, repositorios
* Jekyll / MkDocs: Generación de documentación estática
* Características: Despliegue rápido, versiones reproducibles, accesibles desde navegador

**CI/CD y DevOps**
* Jenkins / GitLab Runner: Automatización de pipelines
* Registry: Almacén de imágenes Docker
* Prometheus / Grafana: Monitorización
* Elasticsearch / Kibana / Logstash: Logging y análisis de logs
* Características: Automatización, visualización centralizada, gestión de registros y métricas

**Big Data y Analytics**
* Jupyter, Apache Spark, Apache NiFi, Kafka, Drill
* Características: Procesamiento de datos, streaming, warehousing; reproducible y escalable en contenedores.

**Machine Learning / AI**
* TensorFlow / PyTorch / Jupyter ML notebooks
* MLflow / Kubeflow: MLOps y gestión de pipelines.
* Características: Uso de GPU, entornos reproducibles, aislamiento de dependencias.

**Servidores de Juegos**
* Minecraft, Counter-Strike, Terraria
* Características: Aislamiento de instancias, fácil despliegue en cualquier servidor, control de puertos de red.

**Aplicaciones de Comunicación**
* Rocket.Chat, Mattermost, Mailserver, Jitsi
* Características: Chat, email y videoconferencia listos para producción, escalables y reproducibles.

**Almacenamiento y Compartición de Archivos**
* Nextcloud, OwnCloud, FTP/SFTP, Syncthing
* Características: Persistencia de datos, acceso multiusuario, sincronización y respaldo.

**Seguridad y Redes**
* VPNs: OpenVPN, WireGuard
* Proxy reverso: Nginx, Traefik
* WAF: ModSecurity
* Características: Aislamiento de tráfico, protección de aplicaciones, configuración reproducible

**CMS y E-commerce**
* CMS: WordPress, Drupal, Joomla, Ghost
* E-commerce: Magento, PrestaShop, WooCommerce
* Características: Instalación rápida, portable, escalable y consistente

**Diseño y Multimedia**
* GIMP, FFmpeg, Plex, Jellyfin
* Características: Edición de imágenes, transcodificación y streaming dentro de contenedores, sin instalar software local.

**Búsqueda e Indexación**
* Elasticsearch, Solr, Manticore, Crawlers
* Características: Motores de búsqueda y crawling reproducibles, fáciles de escalar

**IoT**
* MQTT, Home Assistant, Node-RED
* Características: Gestión de sensores y automatización doméstica, entornos reproducibles

**Sistemas de Gestión**
* ERP: Odoo
* CRM: SuiteCRM
* Project Management: Wekan, Redmine
* Características: Sistemas completos contenibles, aislados y portables

**Testing y QA**
* Selenium, SonarQube
* Características: Automatización de pruebas y análisis de calidad en entornos idénticos a producción

**Backend para Aplicaciones Móviles**
* Supabase, Gotify, Parse Server
* Características: Bases de datos, push notifications, Backend-as-a-Service

**Ejemplo de Stack Completo**
* Frontend (Node.js)
* Backend (Python)
* Base de datos (PostgreSQL)
* Cache (Redis)
* Cola de mensajes (RabbitMQ)
* Monitorización (Prometheus)
* Visualización (Grafana)
* Logs (Elasticsearch)
* Características: Multi-servicio, reproducible, escalable, aislado.

**Otros Tipos de Contenedores**
* Podman: Alternativa a Docker, rootless, daemonless, compatible con Docker
* LXC/LXD: Contenedores de sistema completo, soporta systemd
* containerd: Runtime de bajo nivel para Kubernetes y Docker
* CRI-O: Runtime ligero, nativo para Kubernetes
* Kata Containers / gVisor: Contenedores seguros, alto aislamiento, multi-tenant
* Firecracker: MicroVMs ultraligeras, serverless, rápido y seguro
* Windows Containers: Para aplicaciones Windows y .NET legacy
* systemd-nspawn / OpenVZ: Contenedores ligeros o de sistema completo

* 🔌 ***Escribe una guía de usuario con los pasos claves para desplegar una aplicación web en contenedores.***

En el siguiente enlace, encontramos la guía usada en esta actividad. Esta se haya en mi Gitbook personal bajo el título de "Docker Compose Final".

Link: https://lagar.gitbook.io/lagar/servicios-de-red/docker-compose-final

## 📦 Especificaciones de los archivos principales 

### 🕹️ Dockerfile
### 🕹️ docker-compose.yaml
### 🕹️ default.conf

## 💽 Incidencias técnicas y sus soluciones


