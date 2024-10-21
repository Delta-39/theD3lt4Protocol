# Nmap

Nmap (Network Mapper) es una herramienta de código abierto para el escaneo y mapeo de redes, utilizada principalmente por profesionales de la seguridad para identificar dispositivos y servicios en una red. Nmap es extremadamente versátil y puede realizar desde escaneos básicos de puertos hasta detección avanzada de sistemas operativos y servicios.

## ¿Para qué se usa Nmap?

Nmap se usa para:

- Descubrir dispositivos activos en una red.
- Identificar puertos abiertos en hosts.
- Determinar servicios y versiones de servicios que se ejecutan en un puerto específico.
- Detectar el sistema operativo de un host.
- Auditar la seguridad de redes y dispositivos.

## Instalación de Nmap

Para instalar Nmap en diferentes sistemas operativos, podes usar los siguientes comandos:

- **En Linux (Debian/Ubuntu):**
  ```bash
  sudo apt update
  sudo apt install nmap
  ```
- **En Linux (CentOS/Fedora/RHEL)::**
  ```bash
  sudo yum install nmap
  ```
- **En macOS (usando Homebrew):**
  ```bash
  brew install nmap
  ```
- **En windows:** Podes descargar el instalador desde [aca](https://nmap.org/download.html).

## Comandos más usados de Nmap

### 1. Escaneo Básico de Puertos

Este comando escanea los 1000 puertos más comunes de un host o dirección IP.

  ```bash
  nmap <ip/dominio>
  ```

**Explicación**: Realiza un escaneo básico, identificando los puertos abiertos en la dirección IP o dominio especificado.

### 2. Escaneo de Todos los Puertos

Para escanear todos los puertos (0-65535) de un host, podes usar:

  ```bash
  nmap -p- <ip/dominio>
  ```

**Explicación**: Escanea todos los puertos en lugar de los predeterminados, proporcionando una visión más completa de los servicios que están corriendo.

### 3. Escaneo de Puertos Específicos

Puedes especificar un rango o lista de puertos a escanear.

  ```bash
  nmap -p 80,443,22 <ip/dominio>
  ```

**Explicación**: Escanea solo los puertos indicados (en este caso, los puertos 80, 443 y 22).

### 4. Escaneo con Detección de Sistema Operativo

Para intentar detectar el sistema operativo de un host:

  ```bash
  nmap -O <ip/dominio>
  ```

**Explicación**: Realiza un escaneo de huellas digitales para intentar identificar el sistema operativo que corre en el host.

### 5. Detección de Versiones de Servicios

Este comando intenta determinar la versión de los servicios corriendo en los puertos abiertos.

  ```bash
  nmap -sV <ip/dominio>
  ```

**Explicación**: Identifica las versiones de los servicios que se ejecutan en los puertos detectados como abiertos.

### 6. Escaneo de Red Completa

Para escanear una red completa, usa un rango de IPs:

  ```bash
  nmap 192.168.1.0/24
  ```

**Explicación**: Escanea todos los dispositivos dentro del rango de IPs especificado, ideal para descubrir todos los hosts activos en una subred.

### 7. Escaneo Silencioso (Modo Sigiloso)

El modo sigiloso evita ser detectado por firewalls y sistemas de detección de intrusos.

  ```bash
  nmap -sS <ip/dominio>
  ```

**Explicación**: Realiza un escaneo SYN, conocido como escaneo "semiabierto" o "stealth", que es menos probable que sea registrado.

### 8. Escaneo para Detectar Hosts Activos (Ping Scan)

Para detectar qué hosts están activos en una red:

  ```bash
  nmap -sn <rango de IPs>
  ```

**Explicación**: Nmap solo realiza un ping a cada dirección IP para ver si está activa sin escanear puertos.

## Otras Maneras de Usar Nmap

### 1. Detección de Vulnerabilidades

Nmap puede ser usado junto con scripts NSE (Nmap Scripting Engine) para detectar vulnerabilidades

  ```bash
  nmap --script=vuln <ip/dominio>
  ```

**Explicación**: Utiliza scripts NSE para realizar un escaneo en busca de vulnerabilidades conocidas.

### 2. Escaneo de Red Inversa (Reverse DNS)

Para realizar un escaneo inverso de DNS y obtener nombres de dominio de los hosts

  ```bash
  nmap -sL <rango de IPs>
  ```

**Explicación**: Muestra una lista de hosts y sus respectivos nombres de dominio.

### 3. Descubrimiento de Dispositivos IoT

Con Nmap también se pueden descubrir dispositivos IoT en la red

  ```bash
  nmap --script=banner <ip/dominio>
  ```

**Explicación**: Captura los banners de los servicios corriendo, lo cual puede revelar información sobre dispositivos IoT.

### 4. Escaneo de Puertos con Tiempos de Espera Ajustados

Para ajustar la velocidad del escaneo (útil para evitar bloqueos)

  ```bash
  nmap -T4 <ip/dominio>
  ```

**Explicación**: Ajusta la velocidad de escaneo, donde -T4 es rápido, pero puede ser detectado. Otros valores como -T1 son más lentos pero discretos.

### 5. Escaneo de Firewall y Detección de Reglas

Para identificar las reglas de un firewall y entender cómo maneja ciertos paquetes:

  ```bash
  nmap -sA <ip/dominio>
  ```

**Explicación**: Este escaneo puede determinar si un firewall está presente y cómo trata el tráfico.



