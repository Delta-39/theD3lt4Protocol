# :detective: Tomcat Lab

Esta guia es una ayuda para la resolucion del laboratorio [Tomcat Lab](https://cyberdefenders.org/blueteam-ctf-challenges/tomcat-takeover/) de cyberdefenders.

## Escenario

Nuestro equipo del SOC ha detectado actividad sospechosa en uno de los servidores web dentro de la intranet de la empresa. Para obtener una comprensión más profunda de la situación, el equipo ha capturado el tráfico de red para su análisis. Este archivo pcap podría contener una serie de actividades maliciosas que han resultado en el compromiso del servidor web Apache Tomcat. Necesitamos investigar este incidente más a fondo.

### Q1 - Direccion IP origen escaneo de puertos

La logica que implemente en este caso fue, al tratarse de una escaneo de red, la misma direccion ip estaria haciendo solicitudes a un mismo destino, pero alternando los puertos.

![tomcat1](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat1.png)

Podemos observar la conversacion entre dos direcciones ip, una es la nuestra **`10.0.0.112`** que esta siendo consultada por muchsisimos puertos por parte de la direccion **`14.0.0.120`**.

### Q2 - Posicion geografica direccion IP

Con la direccion ip ya encontrada, es solo cuestion de ir al sitio de [What's my IP address?](https://whatismyipaddress.com/) y colocar la direccion ip para obtener la ciudad del atacante.

![tomcat2](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat2.png)

### Q3 - Puerto para acceder a la pagina web

Los puertos para acceder a la web son generalmente **`80`** para http y **`443`** para https, estos estan dentro de los puertos conocidos comos **`Well-Known Ports`** (puertos bien conocidos). Comunemnete se usa como puerto alternativo al 80, el 8080.

Igualmente para este analisis, podes filtrar por solicutedes `GET` que sigan con una respuesta `200`, dentro de aca buscar algun endpoint que sea **`admin`**, o en este caso **`manager`**

![tomcat3](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat3.png)

### Q4 - Herramienta usada para enumaracion de directorios.

Si filtramos por trafico `http` y vemos el trafico filtrado, veremos que hay muchas solicitudes a endpoints que devuelven una respuesta `404`, esto claramente es un indicio por parte de un atacante de que esta haciendo una enumeración de directorios (hay mas paremtros, como el tiempo entre solicitudes, etc)
Como encontrar la herramienta usada?, tan simple como agarrar cualquiera de estas solicitudes y ver que `user-agent` esta usando. En este caso para verlo mas claro yo lo que hice fue realizar un seguimiento de la conversacion de una de las solicitudes.

??? note "¿ Que es el user agent?"
    Es una cadena de texto que identifica las características del cliente, como el tipo de navegador, versión, sistema operativo y otros detalles. Se utiliza para que el servidor pueda personalizar el contenido que esta enviando.

![tomcat4](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat4.png)

### Q5 - Directorio del administrador

Recordemos que ya detectamos una enumeración de directorios, por lo que deberiamos buscar las respuestas con codigo `200`, a continuacion nos daremos cuenta que esta intentando ingresar a una carpeta de nombre `manager`, seguido de varios intentos con codigo `401`, lo que concicide con la respuesta que estamos buscando.

![tomcat5](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat5.png)

### Q6 - Creedenciales para logueo exitoso

Wireshark nos provee una herramienta que, si bien no funciona la totalidad de las veces , para comunicaciones entre protocolos que no son cifrdaos (en este caso tenemos `http`), nos permite ver las credenciales usadas. Gracias a esto solo busque el acceso que conlleve en una respuesta `200`y listo.

![tomcat7](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat7.png)

### Q7 - Nombre del archivo subido

Para buscar el archivo, lo que hice fue buscar paquetes en los que tengan un apartado mas de tipo `MIME`, esto quiere decir que se estan subiendo archivos comprendidos en este formato (esto se usa cuando en un formulario normal web se quieren enviar imagenes o algun otro archivo que no sea texto.). Encontramos el paquete y veremos lo siguiente

![tomcat6](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat6.png)

### Q7 - Codigo de la reverse shell

Este me costo un poquito mas, ya que para ver lo que hizo el atancante en mi servidor tengo qu hacer seguimiento a la conversacion de un paquete `tcp`, la problematica es, ¿ Que paquete ?

Sigamos la logica, una vez el archivo de la reverse shell, deberiamos hacer el seguimiento la conversacion tcp que siga.

![tomcat8](../../assets/Cyberdefnders/Network%20Forensics/tomcat%20lab/tomcat8.png)

Y con esto terminamos este laboratorio.









