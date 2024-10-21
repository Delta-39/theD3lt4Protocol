# Wireshark

**Wireshark** es una herramienta de análisis de tráfico de red ampliamente utilizada en ciberseguridad y administración de redes. Es capaz de capturar y analizar paquetes de datos en tiempo real, lo que permite inspeccionar lo que está ocurriendo en una red a nivel de detalle. Es utilizada por profesionales de redes, hackers éticos y analistas de seguridad para solucionar problemas, detectar vulnerabilidades y monitorear la actividad de la red.

---
## Instalación de Wireshark

Wireshark está disponible para varios sistemas operativos. Aquí te dejo los comandos para instalarlo en diferentes plataformas:

- **En Linux (Debian/Ubuntu):**
```bash
sudo apt update
sudo apt install wireshark
```
>Durante la instalación, se te preguntará si los usuarios no root deben poder capturar paquetes. Elige Sí si quieres habilitar esta opción.
- **En macOS (usando Homebrew)::**
```bash
brew install wireshark
```
- **En Windows:** Puedes descargar el instalador desde [aca](https://www.wireshark.org/download.html) y seguir las instrucciones del instalador.

---
## Interfaz y Funciones Básicas de Wireshark

Cuando abres Wireshark, te vas a encontrar con una lista de interfaces de red que podess seleccionar para capturar tráfico. A continuación, describimos las funciones clave:

### 1. Captura de Tráfico de Red

Una vez seleccionada una interfaz, simplemente presiona el botón Start (Iniciar) para comenzar a capturar el tráfico en tiempo real.

### 2. Filtrado de Paquetes

Los filtros en Wireshark son esenciales para reducir la cantidad de datos que ves y enfocarte en lo que necesitas analizar. Existen dos tipos de filtros:

**Filtros de Captura**: Se aplican antes de comenzar la captura, limitando los paquetes que Wireshark almacenará.

**Filtros de Visualización**: Filtran los paquetes ya capturados para mostrar solo los que te interesan.

Ejemplos de filtros comunes:

- **Mostrar solo tráfico HTTP:**
```bash
http
```
- **Filtrar paquetes por dirección IP:**
```bash
ip.addr == 192.168.1.1
```
- **Mostrar solo tráfico TCP o UDP en el puerto 80:**
```bash
tcp.port == 80 || udp.port == 80
```

### 3. Decodificación de Protocolos
Wireshark puede interpretar y decodificar una gran cantidad de protocolos, como HTTP, DNS, FTP, SSL/TLS, entre otros. Puedes analizar los detalles de cada paquete capturado haciendo clic en el mismo, lo que mostrará información como:

- Dirección IP de origen y destino.
- Puerto de origen y destino.
- Protocolo usado (TCP, UDP, etc.).
- Datos de la capa de aplicación.

### 4. Exportación de Capturas
Es posible guardar tus capturas de tráfico para analizarlas más tarde o compartirlas con otros:

Ve a File > Export Packet Dissections > As CSV para exportar los datos en un formato de hoja de cálculo.
También puedes guardar el archivo en formato PCAP (formato de captura de paquetes estándar) para abrirlo luego.

---
## Comandos y Filtros Útiles en Wireshark
Wireshark ofrece una amplia gama de filtros que te permiten analizar redes con precisión. Estos son algunos de los más comunes:

- **Capturar tráfico de un puerto específico:**
```bash
tcp.port == 443
```
- **Filtrar por protocolo (por ejemplo, DNS):**
```bash
dns
```
- **Filtrar tráfico proveniente de una dirección IP específica**
```bash
ip.src == 10.0.0.1
```
- **Mostrar solo paquetes que contengan errores:**
```bash
tcp.analysis.flags
```
---
## Análisis de Tráfico en Tiempo Real

### 1. Inspeccionar Sesiones HTTP y FTP

Puedes capturar y analizar sesiones HTTP para ver cómo un cliente y un servidor interactúan. Wireshark te permite ver las solicitudes y respuestas HTTP con todos sus encabezados y contenido.

### 2. Detección de Credenciales no Cifradas

Una de las funciones clave en el hacking ético es la captura de credenciales en redes no seguras. Si capturas tráfico en un entorno donde se usa FTP (sin cifrar) o HTTP, podrías ver las credenciales de inicio de sesión en texto plano. Por ejemplo, usando el filtro:

```bash
ftp.request.command == "USER" || ftp.request.command == "PASS"
```

### 3. Análisis de Redes Inalámbricas

Combinando Wireshark con herramientas como *`Aircrack-ng`*, podes capturar tráfico en redes inalámbricas y analizar los paquetes para detectar problemas o realizar auditorías de seguridad.

---
## Casos Prácticos en Hacking Ético

### 1. Captura de Credenciales en HTTP

Un caso típico de análisis con Wireshark es capturar tráfico en redes inseguras para detectar credenciales que viajan en texto plano. Capturando paquetes HTTP, puedes inspeccionar las solicitudes de inicio de sesión y extraer datos confidenciales.

### 2. Análisis de Ataques de Man-in-the-Middle (MitM)

Wireshark es útil para detectar ataques de Man-in-the-Middle. Si sospechas de que un ataque MitM está ocurriendo, puedes monitorear el tráfico y buscar señales de duplicación de tráfico o paquetes maliciosos inyectados.

### 3. Identificación de Escaneos de Red

Wireshark también es útil para identificar intentos de escaneo de red con herramientas como Nmap. Un patrón de tráfico inusual en una red, como muchos paquetes enviados a múltiples puertos en sucesión, puede indicar un escaneo en curso.

----
## Exportación y Generación de Informes

Wireshark permite guardar las capturas y generar informes detallados sobre el tráfico capturado. Para guardar una captura:

- Ve a File > Save As y elige el formato PCAP o PCAPNG.

También podes exportar los resultados como archivos CSV, JSON, o XML, lo que es útil para compartir los datos con otros analistas o para importar los resultados a otras herramientas.