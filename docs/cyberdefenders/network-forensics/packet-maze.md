# :detective: Packet Maze

Esta guia es una ayuda para la resolucion del laboratorio [Packet Maze](https://cyberdefenders.org/blueteam-ctf-challenges/packetmaze/) de cyberdefenders.

## Escenario

Un servidor interno de la empresa ha sido marcado por una actividad de red inusual, con múltiples conexiones salientes hacia una dirección IP externa desconocida. El análisis inicial sugiere una posible exfiltración de datos. Investiga los registros de red proporcionados para determinar el origen y el método de compromiso.

### Q1 - Contraseña FTP

Aca simplemnte debemos (una vez abierta la capura pcap de wireshark) aplicar el filtro **`ftp`**, nos pararemos en el primer paquete y solo tendremos que dar clic derecho y seguir la conversacion. Podremmos encontrar la pass del servicio en texto plano, ya que este servicio no transmite la informacion cifrafa

![packetmaze1](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/1.png)

??? note "SFTP"
    SFTP (Secure File Transfer Protocol) es un protocolo de transferencia de archivos que opera sobre una conexión segura mediante SSH. A diferencia de FTP, cifra tanto los datos como las credenciales, lo que protege la información durante la transmisión. Esto lo hace mucho más seguro frente a ataques o interceptaciones. Por eso, SFTP es preferido en entornos donde la seguridad es fundamental.

### Q2 - Dirección IP Servidor DNS

Para esto solamente tenemos que aplicar el filtro de **`dns`**, y utilizando un poco la logica ya en la pregunta anterior podemos identificar la direccion ip , a partri de ahi es ver quien responde la consulta de dns.

### Q3 - Paquete 15174

Aplicamos el filtro por paquete (nos va a dar un paquete de dns), y tan solo tenemos que ver cual fue la querie que consulto el usuario.

![packetmaze2](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/2.png)

### Q4 - Cantidad de paquetes UDP

Ahora nos pide un cantidad especifico de paquetes **`udp`**, entre dos direcciones ips , para ver esto se puede acceder al apartado de `"conversaciones"`, este apartado contiene todos los paquetes transmitidos entre las diferentes ips, se puede aplicar filtros por protocol y asi tener un analisis mas preciso.

![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/3.png)

### Q5 - MAC ip en investigacion

Ya sabemos la direccion ip que estamos investigando, abriremos cualquier paquete que tenga esta ip (ya se en origen o destino), bajaremos a nivel capa 2 y ahi veremos las direcciones mac tanto de origen como de destino

### Q6 - Modelo de camara 

Wireshark permite que dentro de sus capturas pcap, si se deacargo algun tipo de archivo, ese archivo puede quedar embebido dentro de los paquetes capturados. Gracias a esto, Wireshark brinda la posibilidad de reconstruir y extraer esos archivos directamente desde la captura, permitiendo su análisis posterior. Esta funcionalidad es muy útil en investigaciones forenses, ya que permite recuperar evidencia digital sin necesidad de acceso directo al sistema comprometido. Veremos en archivo y abriremos el apartado de objetos, descargaremos el deseado e iremos a ver sus metadatos

![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/4.png)
![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/5.png)

### Q7 - Llave publica del servidor

Durante el establecimiento de una conexión segura a través de TLS (Transport Layer Security), cliente y servidor realizan un handshake para acordar cómo protegerán la comunicación. En las versiones modernas de TLS (especialmente con cifrados de tipo Diffie-Hellman efímero como ECDHE), el servidor genera una clave pública efímera, que se comparte con el cliente como parte del proceso de negociación de claves.  

Dicho esto aplicaremos el filtro de `tls.handshake.session.id` , colocando la id de session y encontraremos dentro del paquete la llave publica que estamos buscando

![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/6.png)

### Q8 - Llave publica del servidor

Sabemos que el `Client random `se genera al momento del `Client hello`, sabemos que establecio una conexion con **`Protonmail.com`**, entonces debemos filtrar el protocol tls pero con **`server_name == protonmail.com`**, buscaremos el primer paquete y dentro del Client hello encontraremos el valor que buscamos

??? note "Client Random"
    Durante el handshake de una conexión TLS 1.3, el cliente (por ejemplo, tu navegador) envía un mensaje llamado ClientHello al servidor.
    Este mensaje contiene un valor aleatorio de 32 bytes llamado client random, que se utiliza como parte del proceso de generación de claves para cifrar la sesión.

![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/7.png)

### Q9 - Pais fabricante MAC FTP

Es cuestion de obtener la MAC como explique anteriormente, e ir a buscarla en google

![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/8.png)

### Q9 - Carpeta no estandar

Volvermos al principio, a la conversacion que vimos en un principio e ir avanzando en las secuencias hasta que veremos la secuencias de la conversacion que tuvo la maquina que estamos invetigando con el servidor ftp, en una de la secuencias se lista el contenido y alli podremos ver lo siguiente

![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/9.png)

### Q9 - URL visitada - IP 104.21.89.171

Colocaremos en el filtro la direccion en cuestion, y es cuestion de ver el encabezado de host del protocol http

![packetmaze3](../../assets/Cyberdefnders/Network%20Forensics/Packet%20Maze/10.png)

Y con esto terminamos este write-up