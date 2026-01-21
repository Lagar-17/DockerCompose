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

Docker Registry
* Repositorio para almacenar y distribuir imágenes Docker.

Docker Compose
* Define y ejecuta aplicaciones multicontenedor.

Jenkins
* Automatiza CI/CD ejecutando jobs en contenedores Docker.

GitLab Runner
* Ejecuta pipelines CI/CD usando contenedores Docker.

Prometheus
* Monitoriza métricas de contenedores y servicios.

Grafana
* Visualiza métricas de Docker y Kubernetes.

Nginx
* Proxy reverso y servidor web en contenedores.

Traefik
* Proxy reverso con descubrimiento automático de contenedores.

Kubernetes
* Orquesta y escala contenedores Docker.

containerd
* Runtime que ejecuta contenedores (usado por Docker y Kubernetes).

* 🔌 ***Escribe una guía de usuario con los pasos claves para desplegar una aplicación web en contenedores.***

En el siguiente enlace, encontramos la guía usada en esta actividad. Esta se haya en mi Gitbook personal bajo el título de "Docker Compose Final".

Link: https://lagar.gitbook.io/lagar/servicios-de-red/docker-compose-final

## 📦 Especificaciones de los archivos principales 

### 🕹️ Dockerfile

**Ubicación:** ~/test-dcompose-lenvel/Dockerfile

**¿Qué hace?**

Este archivo define cómo construir la imagen personalizada de PHP que usará tu aplicación. Es como una "receta" que Docker sigue para preparar el entorno PHP necesario.

**Contenido:**

<img width="1200" height="216" alt="image" src="https://github.com/user-attachments/assets/da48519d-d33f-434f-ab34-9f8661840e04" />


**Explicación línea por línea:**

* FROM php:8-fpm: Utiliza como base la imagen oficial de PHP versión 8 con PHP-FPM (FastCGI Process Manager). FPM es necesario para que Nginx pueda comunicarse con PHP.
* RUN docker-php-ext-install mysqli pdo pdo_mysql: Instala tres extensiones de PHP que son fundamentales para conectarse a MySQL:
   * mysqli: Extensión mejorada de MySQL para PHP
   * pdo: PHP Data Objects, interfaz para acceder a bases de datos
   * pdo_mysql: Driver específico de PDO para MySQL
* WORKDIR /var/www/lenvel: Establece el directorio de trabajo dentro del contenedor donde residirán tus archivos PHP.

**¿Por qué es necesario?**

PHP por defecto no incluye las extensiones de MySQL. Sin este Dockerfile, tu aplicación no podría conectarse a la base de datos, generando el error: "Call to undefined function mysqli_connect()".

### 🕹️ docker-compose.yaml

**Ubicación:** ~/test-dcompose-lenvel/docker-compose.yaml

**¿Qué hace?**

Es el archivo maestro que orquesta todos los servicios de tu aplicación. Define qué contenedores se crean, cómo se comunican entre sí, qué puertos exponen y qué volúmenes comparten.

**Contenido:**

<img width="1206" height="734" alt="image" src="https://github.com/user-attachments/assets/2cf0f41e-d374-4be6-9cd5-03ec8a435040" />


**Explicación de cada servicio:**

Servicio db (MySQL)

* Propósito: Base de datos que almacena los cursos de Lenvel
* image: mysql:8.0: Usa la imagen oficial de MySQL versión 8.0
* ports: "3307:3306": Mapea el puerto interno 3306 de MySQL al puerto 3307 de tu máquina (para evitar conflictos si ya tienes MySQL instalado)
* environment: Variables de entorno que configuran MySQL automáticamente
   * Crea el usuario root con contraseña 1234
   * Crea la base de datos lenvel_cursos
* volumes: Persiste los datos en tu disco duro, así no pierdes información al reiniciar el contenedor
* restart: unless-stopped: Reinicia automáticamente si falla (excepto si lo paras manualmente)

Servicio app (PHP-FPM)

* Propósito: Procesa los archivos PHP de tu aplicación
* build: .: Construye la imagen usando el Dockerfile del directorio actual
* volumes:
   * Monta tu carpeta Lenvel dentro del contenedor para que PHP pueda leer tus archivos
   * Guarda logs de PHP en tu máquina para depuración
* depends_on: db: No inicia hasta que MySQL esté corriendo
* Puerto: No expone puertos externos, se comunica internamente con Nginx en el puerto 9000

Servicio nginx (Servidor Web)

* Propósito: Servidor web que recibe las peticiones HTTP y las dirige a PHP
* ports: "86:80": Accesible desde http://localhost:86
* volumes:
   * Acceso a los archivos web (HTML, CSS, imágenes)
   * Lee su configuración desde default.conf
   * Guarda logs de acceso y errores
* depends_on: app: Espera a que PHP esté listo antes de iniciar

Servicio phpmyadmin

* Propósito: Interfaz gráfica para administrar la base de datos
* ports: "8081:80": Accesible desde http://localhost:8081
* PMA_ARBITRARY: 1: Permite conectarse a cualquier servidor MySQL
* Útil para: Crear tablas, ejecutar SQL, ver datos sin usar terminal

Network lenvel-network

* Propósito: Red privada que conecta todos los contenedores
* driver: bridge: Tipo de red que permite comunicación interna
* Importante: Gracias a esta red, PHP puede conectarse a MySQL usando el nombre db en lugar de una IP

### 🕹️ default.conf

**Ubicación:** ~/test-dcompose-lenvel/Lenvel/nginx/conf.d/default.conf

**¿Qué hace?**

Configura cómo Nginx debe manejar las peticiones HTTP, específicamente cómo procesar archivos PHP y servir archivos estáticos.

**Contenido:**

<img width="1205" height="508" alt="image" src="https://github.com/user-attachments/assets/05cce308-af33-42a0-b262-f8f1c31935fb" />

**Explicación detallada:**

Bloque server

* listen 80;: Nginx escucha en el puerto 80 (dentro del contenedor)
* root /var/www/lenvel;: Carpeta raíz donde están tus archivos
* index lenvel.php index.html;: Archivos que Nginx busca por defecto al acceder a una carpeta

Bloque location ~ \.php$ (para archivos PHP)

Este bloque maneja todas las URLs que terminan en .php:
* try_files $uri =404;: Verifica que el archivo PHP existe, si no, devuelve error 404
* fastcgi_pass app:9000;: CRÍTICO - Envía la petición al contenedor app (PHP-FPM) en el puerto 9000. Aquí es donde ocurre la magia:  Nginx delega el procesamiento PHP
* fastcgi_index lenvel.php;: Archivo por defecto si solo se especifica un directorio
* include fastcgi_params;: Incluye parámetros estándar de FastCGI
* fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;: Le dice a PHP la ruta exacta del archivo a ejecutar

Bloque location / (para todo lo demás)

Maneja archivos estáticos como CSS, imágenes, JS:
* try_files $uri $uri/ /lenvel.php?$query_string;:
   * Intenta servir el archivo directamente ($uri)
   * Si no existe, intenta como directorio ($uri/)
   * Si tampoco, redirige a lenvel.php con los parámetros

## 💽 Incidencias técnicas y sus soluciones

* Error 1: Pensar que los datos del contenedor eran persistentes

Uno de los primeros errores que cometí fue creer que los datos creados dentro de un contenedor se guardaban automáticamente. Después de eliminar un contenedor, me di cuenta de que toda la información había desaparecido. Aprendí que la capa de escritura del contenedor es efímera y que, si quiero conservar datos, debo usar volúmenes o bind mounts.

* Error 2: No mapear correctamente los puertos

En varios casos la aplicación funcionaba dentro del contenedor, pero no podía acceder a ella desde el navegador. El problema era que no había mapeado los puertos correctamente. Aprendí que Docker aísla la red del contenedor y que es necesario exponer los puertos para poder acceder a los servicios.

* Error 3: Ejecutar varios servicios en un mismo contenedor

Intenté ejecutar más de un servicio dentro del mismo contenedor para simplificar la configuración, pero esto complicó el mantenimiento y la depuración. Aprendí que la filosofía de Docker es ejecutar una sola aplicación por contenedor.

* Error 4: Ejecutar contenedores como root

Al principio no presté atención al usuario con el que se ejecutaban los contenedores y dejé que todo corriera como root. Después entendí que esto supone un riesgo de seguridad y que es mejor ejecutar las aplicaciones con usuarios sin privilegios.

* Error 5: Confundir imágenes con contenedores

En algunos momentos confundí el concepto de imagen con el de contenedor, lo que me llevó a errores al eliminar recursos o a problemas de espacio en disco. Con el tiempo comprendí que las imágenes son plantillas y los contenedores son instancias en ejecución.

* Error 6: No limpiar recursos que ya no se usan

Conforme avanzaba en la actividad, fui acumulando contenedores detenidos, imágenes antiguas y volúmenes sin uso. Esto ocupó espacio innecesario en el disco. Aprendí que es importante hacer limpiezas periódicas para mantener el entorno ordenado.

## 🧩 Conclusión

Con esta actividad he aprendido a desplegar una aplicación web completa usando Docker Compose, integrando varios servicios (Nginx, PHP, MySQL y phpMyAdmin) y haciendo que se comuniquen entre sí. He comprendido la importancia de usar correctamente redes, volúmenes y variables de entorno, así como de configurar bien los archivos de conexión.
También he aprendido a construir imágenes con Dockerfile, levantar y gestionar contenedores, y a detectar y solucionar errores. 

**Resultado final de la web y la base de datos:**

<img width="1212" height="714" alt="image" src="https://github.com/user-attachments/assets/22e8628c-d6f0-4572-939a-4bf45b9b2511" />

<img width="1211" height="717" alt="image" src="https://github.com/user-attachments/assets/a4ba2dd1-bdf2-4bf0-af5f-5cb82db19732" />
