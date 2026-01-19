# DOCKER COMPOSE - PROYECTO FINAL

## Preguntas

* ***¿Qué son los contenedores de docker?***

Los contenedores de Docker son unidades de software ligeras, portables y autosuficientes que empaquetan una aplicación junto con todas sus dependencias (librerías, configuraciones, archivos de sistema) necesarias para ejecutarse.

 * Ejemplo práctico - Ejecutar un servidor web Nginx en un contenedor: docker run -d -p 80:80 nginx
    * El contenedor incluye: Nginx instalado, todas las librerías necesarias, configuración por defecto, sistema de archivos mínimo

Características principales:
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

* ***¿Qué diferencias hay entre los contenedores de docker y los lxc?***
s

* ***¿Cuál es la diferencia entre una imagen y un contenedor en docker?***
s

* ***¿Qué sucede con los datos cuando un contenedor se elimina?***
s

* ***¿Cuáles son las ventajas de utilizar contenedores de docker?***
s

* ***¿Qué tipo de aplicaciones y servicios se pueden desplegar con docker?***
s

* ***¿Qué otros tipos de contenedores existen además de docker?***
s

* ***Escribe una guía de usuario con los pasos claves para desplegar una aplicación web en contenedores.***
s

## Especificaciones de los archivos principales

### Dockerfile
### docker-compose.yaml
### default.conf

## Incidencias técnicas y sus soluciones

## Enlace a Github 

A continuación, dejo el enlace donde se podrá encontrar la guía completa seguida para la correcta realización de este trabajo.
