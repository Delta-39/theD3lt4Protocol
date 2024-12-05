# Guia de resolucion Webstrike Lab

Esta guia esuna ayuda para la resolucion del laboratorio [Webstrike Lab](https://cyberdefenders.org/blueteam-ctf-challenges/webstrike/) de cyberdefenders.¡Manos a la obra!

## Escenario

Se descubrió una anomalía dentro de la intranet de nuestra empresa, ya que el equipo de Desarrollo encontró un archivo inusual en uno de nuestros servidores web. Sospechando de una posible actividad maliciosa, el equipo de redes ha preparado un archivo pcap con tráfico de red crítico para que el equipo de seguridad lo analice, y se te ha asignado la tarea de realizar dicho análisis.

### Q1 Understanding the geographical origin of the attack aids in geo-blocking measures and threat intelligence analysis. What city did the attack originate from?

Para esto tenes que primeramente identificar la direccion `IP` del atacante, yo lo que hice para poder identificar dicah direccion fue aplicar el filtro `http` en wireshark, una vez hecho esto, nos damos cuenta que la primer solicitud `GET` hecha a nuestro servidor viene con la direccion **`117.11.88.124`**. Esto es debido a que cada vez que se solicita una pagina web, se realiza una solicitud de tipo `GET` para poder obtener los archivos del servidor y poder renderizarlos.

![webstrike1](../../assets/Cyberdefnders/Network%20Forensics/Webstrike/webstrike1.png)

Ahora solamente queda ir a [What is my ip address?](https://whatismyipaddress.com/), ingresar la direccion ip y obtendremos la ciudad originaria de este ataque.

![webstrike2](../../assets/Cyberdefnders/Network%20Forensics/Webstrike/webstrike2.png)

### Q2 Knowing the attacker's user-agent assists in creating robust filtering rules. What's the attacker's user agent?

Aca simplemente debemos ingresar a cualquier paquete con protocolo `http`, abrir el pquete y buscar el encabezado `user-agent`, este encabezado tiene informacion del programa con el cual estamos haciendo la solicitud e informacion de nuestro O.S.

![webstrike3](../../assets/Cyberdefnders/Network%20Forensics/Webstrike/webstrike3.png)

### Q3 We need to identify if there were potential vulnerabilities exploited. What's the name of the malicious web shell uploaded?

Para esta pregunta, filtre las solicitudes http por el metodo `POST`, aplicando el filtro en wireshark `http.request.method == POST`, esto es debido a que si nuestro atacante subio una reverse shell, lo mas probable es que lo haya esto con este metodo.

> El método POST de HTTP se utiliza para enviar datos al servidor, generalmente en el cuerpo de la solicitud, para procesarlos o almacenarlos.

Una vez aplicado el filtro, debemos seguir la secuencia http del paquete, y obtendremos el nombre del archivo

![webstrike4](../../assets/Cyberdefnders/Network%20Forensics/Webstrike/webstrike4.png)

### Q4 Knowing the directory where files uploaded are stored is important for reinforcing defenses against unauthorized access. Which directory is used by the website to store the uploaded files?

Generalmente, una vez que subimos una reverse shell a un servidor, luego debemos hacer una llamada de tipo `GET` a la direccion en la cual subimos la shell para que se ejecute. Entonces siguiendo esta logica, deberiamos fltrar los paquetes por el metodo mencionado y buscar una respuesta **`200`** con el nombre del archivo que encontramos.

![webstrike8](../../assets/Cyberdefnders/Network%20Forensics/Webstrike/webstrike8.png)

### Q5 Identifying the port utilized by the web shell helps improve firewall configurations for blocking unauthorized outbound traffic. What port was used by the malicious web shell?

Esto es tan simple como volver a seguir la secuencia de subida del archivo **`image.jpg.php`**, dentro de esta secuencia podremos observar el codigo `php` de la reverse shell, la misma utiliza `netcat` para realizar la escucha.

![webstrike5](../../assets/Cyberdefnders/Network%20Forensics/Webstrike/webstrike5.png)

### Q6 Understanding the value of compromised data assists in prioritizing incident response actions. What file was the attacker trying to exfiltrate?

Filtraremos los paquetes `tcp` con puerto de destino 8080, aca solamente debemos seguir la trama tcp para ver toda la actividad del atacante dentro de nuestro servidor ubuntu

![webstrike6](../../assets/Cyberdefnders/Network%20Forensics/Webstrike/webstrike6.png)


¡ Y listo ! Espero que te sirva.