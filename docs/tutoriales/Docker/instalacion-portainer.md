# Instalación de Portainer en un Servidor Ubuntu

En este tutorial, aprenderás qué es Portainer y cómo instalarlo en un servidor Ubuntu para simplificar la gestión de tus contenedores Docker.

## ¿Qué es Portainer?

Portainer es una herramienta de gestión de contenedores ligera y fácil de usar, que proporciona una interfaz web intuitiva para gestionar Docker. Con Portainer, puedes administrar contenedores, imágenes, volúmenes y redes sin tener que depender exclusivamente de la línea de comandos.

### Características clave de Portainer

- Implementación y gestión de contenedores, imágenes, volúmenes y redes.
- Gestión de múltiples hosts Docker y clústeres de Swarm.
- Integración con registros de Docker (Docker Hub, registros privados).
- Gestión de usuarios y control de acceso basado en roles.
- Consola de línea de comandos web para acceder a tus contenedores.

## Requisitos Previos

Antes de instalar Portainer, asegúrate de contar con lo siguiente:

- Servidor Ubuntu con al menos **1 GB de RAM** y **1 núcleo de CPU**.
- **Docker** instalado en el servidor Ubuntu.
- Acceso **SSH** al servidor con privilegios de sudo.
- Una **dirección IP estática** configurada en el servidor (opcional pero recomendado).

## Configuración de una IP Estática (Opcional)

Configurar una dirección IP estática facilita el acceso a la interfaz web de Portainer. Sigue estos pasos:

1. **Instalar net-tools** si aún no está instalado:
    ```bash
    sudo apt update
    sudo apt install net-tools
    ```

2. **Identificar el nombre de la interfaz de red** usando el comando `ifconfig`.

3. **Editar el archivo de configuración de Netplan**:
    ```bash
    sudo nano /etc/netplan/00-installer-config.yaml
    ```

4. **Modificar el archivo** para configurar una IP estática:
    ```yaml
    network:
      ethernets:
        enp0s3:
          dhcp4: no
          addresses:
            - 192.168.1.100/24
          gateway4: 192.168.1.1
          nameservers:
            addresses:
              - 8.8.8.8
              - 8.8.4.4
      version: 2
    ```

    > Asegúrate de reemplazar `enp0s3` con el nombre de tu interfaz de red.

5. **Aplicar los cambios**:
    ```bash
    sudo netplan apply
    ```

## Instalación de Portainer en Ubuntu

Una vez que tengas Docker configurado, sigue estos pasos para instalar Portainer:

1. **Conéctate por SSH** a tu servidor Ubuntu.
2. **Crear un volumen** para almacenar los datos persistentes de Portainer:
    ```bash
    docker volume create portainer_data
    ```
3. **Ejecutar el contenedor de Portainer**:
    ```bash
    docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
    ```

    > Este comando crea el contenedor de Portainer, mapeando los puertos y configurando el almacenamiento de datos.

4. **Verificar el estado del contenedor**:
    ```bash
    docker ps
    ```

5. **Acceder a la interfaz web de Portainer**:
   Abre un navegador y ve a `https://<ip_servidor_ubuntu>:9443`.

## Configuración Inicial de Portainer

1. **Establecer la contraseña de administrador** para tu instancia de Portainer y hacer clic en "Create User".
2. **Seleccionar "Gestionar el entorno local"** y hacer clic en "Connect".

¡Listo! Ahora tienes Portainer instalado y funcionando en tu servidor Ubuntu, listo para gestionar tus contenedores Docker.
