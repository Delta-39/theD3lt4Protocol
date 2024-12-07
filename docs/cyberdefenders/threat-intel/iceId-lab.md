# Guia de resolucion IceID Lab

Esta guia esuna ayuda para la resolucion del laboratorio IceID de cuberdefenders, si bien te voy a ayudar a que encuentres las respuestas, no pretendo dartelas. Con esto ultimo aclarado ¡Manos a la obra!

## Escenario

Se identificó un grupo de ciberamenaza por iniciar campañas de phishing a gran escala para distribuir cargas útiles maliciosas adicionales. Las cargas útiles más frecuentemente encontradas fueron IcedID. Se te ha proporcionado un hash de una muestra de IcedID con el propósito de analizar y monitorear las actividades de este grupo de amenaza persistente avanzada (APT).

## Cuestionario

Comenzaremos descargando el archivo que nos provee el laboratorio, el mismo es un archivo que contiene el hash: **`d86405130184186154daa4a5132dd1364ab05d1f14034c7f0a0cda690a91116d`**, con este hash nos dirigiremos a [VirusTotal](https://www.virustotal.com/) y colocaremos el hash obtenido para ver el analisis de dicho archivo.

### Q1 - Nombre del archivo con el hash

Nos iremos a la pestaña `Details`, en la cual tendremos todos los detalles de este archivo. Si vamos un poco hacia abajo encontraremos un divisor con el nombre de `Names`, estos son todos los nombres que se descubrieron digamos, que pudo tener este archivo en cuestion.

![Ice-1](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice1.png)

### Q2 - Nombre del archivo GIF desplegado

A esta respuesta llegue de diferentes maneras, la primera fue yendo a la pestaña `Relations`, una vez alli veremos en `Contacted Urls` varias urls, pero habiendo un archivo de tipo `gif` que se repite en varias de ellas.

![Ice-2](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice2.png)

La otra manera fue, bajando aun mas encontraremos una seccion con el nombre de `Dropped Files`, en donde, si buscamos bien, encontraremos un archivo `.gif` con una pequeña particularidad.

![Ice-3](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice3.png)

### Q3 - Cantidad de dominios descargando el archivo GIF

Para esta pregunta solamente deberemos a ir a un apartado anterior, y contar la cantidad de dominios descargarian el archivo que averiguamos en la pregunta 2

> Pssss => Fijate en donde conseguimos al principio el nombre del archivo.

### Q4 - Hash MD5 del ejecutable descargado

Con esta pregunta tuve que hacer un poquito mas de busqueda (Voy a confesar que use un `hint` jaja). Bueno si hacemos una busqueda en la red social `X`, mas especificamente debemos ir a la siguiente publicacion de [JRoosen](https://x.com/JRoosen/status/1376994339281309699), en la cual tendremos dos links en donde se hace un analisis mas profundo en plataformas como anyrun y triage.

![Ice-4](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice4.png)

En este caso ire a la plataforma de any run en donde obtendremos lo siguiente:

![Ice-5](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice5.png)
![Ice-6](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice6.png)

### Q5 - Registrar utilizado por el actor de amenazas

Para resolver esta pregunta lo que debemos hacer es ir a los dominios asociados a este malware, cada dominio tiene que estar registrado a nombre de alguien, veremos uno en particular que coincide con el formato de la respuesta.

![Ice-7](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice7.png)

### Q6 - Actor de amenazas relacionado

Para resolver esto usaremos el framework de MITRE `ATT&CK`, yo lo que hice fue buscar el nombre del malware para asi saber las `APT` que la usan. Obtuve los siguiente resultados.

![Ice-8](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice8.png)

Encontramos que hay dos `APT` relacionada con con el malware en cuestion

![Ice-9](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice9.png)

### Q7 - Función utilizada para obtener payloads

Ahora lo que debemos encontrar es la funcion de la `API` de windows que usa el malware para descargar el resto de las payloads. Para iremos al link de triage que habiamos encontrado en X. Iremos hasta malware config, source y veremos que hay un codigo. Hay algo que se repita que te llame la atencion?

> Te dejo un recurso para que veas cuales son las funciones mas usadas en malware [MalAPI](https://malapi.io/)

![Ice-10](../../assets/Cyberdefnders/Threat%20Intel/IceID/ice10.png)

Y con esto finalizamos este write up. Espero que te haya servido.
