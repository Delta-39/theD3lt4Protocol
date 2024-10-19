# Primeros pasos con Docker

Docker es una herramienta que permite crear, desplegar y ejecutar aplicaciones en contenedores. Los contenedores son ligeros y portables, lo que hace que Docker sea ideal para el desarrollo de aplicaciones y la gestión de entornos de producción.

## ¿Qué es un contenedor?

Un contenedor es una instancia de una imagen de Docker. Las imágenes contienen todo lo necesario para ejecutar una aplicación: código, dependencias, variables de entorno y configuraciones. Los contenedores permiten que las aplicaciones se ejecuten de forma consistente en diferentes entornos.

## Instalación de Docker

Si aún no tienes Docker instalado, puedes seguir este comando para instalarlo en un sistema Ubuntu:

```bash
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### Comandos básicos de Docker
A continuación, te presento algunos de los comandos más comunes para empezar a trabajar con Docker:

1. **Verificar la instalación de Docker**
    ```bash
    docker --version
    ```
> Este comando muestra la versión de Docker instalada en tu sistema.

2. **Descargar una imagen de Docker**
    ```bash
    docker pull <nombre_imagen>
    ```
Por ejemplo, para descargar la imagen oficial de nginx:

    ```bash
    docker pull nginx
    ```

3. **Listar imágenes descargadas**
    ```bash
    docker images
    ```
> Muestra una lista de todas las imágenes de Docker que tienes en tu sistema.

4. **Ejecutar un contenedor**
    ```bash
    docker run -d -p 80:80 nginx
    ```
>Este comando ejecuta un contenedor en segundo plano (-d) y mapea el puerto 80 del host al puerto 80 del contenedor.

5. **Ver contenedores en ejecución**
    ```bash
    docker ps
    ```
>Lista todos los contenedores que están actualmente en ejecución.

6. **Detener un contenedor**
    ```bash
    docker stop <id_contenedor>
    ```
> Reemplaza <id_contenedor> con el ID o el nombre del contenedor que deseas detener.

7. **Eliminar un contenedor**
    ```bash
    docker rm <id_contenedor>
    ```
>Este comando elimina un contenedor detenido. Asegúrate de detener el contenedor antes de eliminarlo.

### Recursos adicionales

[Documentación oficial de Docker](https://docs.docker.com/)

[Docker Hub: Encuentra imágenes oficiales y de la comunidad](https://hub.docker.com/)

¡Con estos comandos básicos, ya estás listo para comenzar a explorar el mundo de Docker!
