# :open_file_folder: Insider Lab

Esta guia es una ayuda para la resolucion del laboratorio [Insider](https://cyberdefenders.org/blueteam-ctf-challenges/insider/) de cyberdefenders.

## Escenario

Después de que Karen comenzó a trabajar para 'TAAUSAI', empezó a realizar algunas actividades ilegales dentro de la empresa. 'TAAUSAI' te contrató como analista SOC para iniciar una investigación sobre este caso.

Has adquirido una imagen de disco y descubriste que Karen utiliza un sistema operativo Linux en su computadora. Analiza la imagen de disco de la computadora de Karen y responde las preguntas proporcionadas

### Q1 - Distribución utilizado en el host

Para la resolucion de este laboratorio usaremos la herramienta de forensia **`FTK Imager`** , la cual me permite la obtencion de evidencia digital sin alterar los datos originales, y hacer un analisis de esta obtencion. 

En este caso descargaremos una imagen forense de tipo **`AD1`**, la cual cargaremos en esta herramienta.

Al abrir la imagen forense, nos damos cuenta que es una distribucion de `linux`, sabiendo esto nos dirijiremos a la carpeta `boot`, la cual contiene los archivos para el inicio del sistema. Una vez alli abriremos el archivo `grub.cfg` (archivo de configuracion de grub), en donde encontraremos la distribucion del host

![insider1](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider1.png)

### Q2 - Hash access.log

Ya que sabemos que es una distribucion de linux, sabemos que todo lo respectivo a los logs se encuentra en la carpeta **`/var/log`**, una vez en esta carpeta iremos al serivcion en cuestion y encontraremos el archivo `access.log`. Al hacer click en el archivo, si vemos la esquina inferior izquierda de nuestra pantalla veremos uin cuadro que ofrece información general del archivo

![insider2](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider2.png)

### Q3 - Archivo descargado

Esta es bastante simple, como sabemos cada vez que descargamos algo se guarda por default en la carpeta `Downloads`, iremos hasta la carpeta en cuestion y encontraremos un archivo zip con el nombre de `mimikatz`

![insider3](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider3.png)

??? note "Mimikatz"
    Mimikatz es una herramienta de código abierto desarrollada por Benjamin Delpy que permite la extracción de credenciales y otros datos sensibles de sistemas Windows. Es ampliamente conocida en el ámbito de la ciberseguridad porque puede recuperar contraseñas en texto plano, hashes de contraseñas, PINs y tickets Kerberos de la memoria de un sistema.

### Q4 - Archivo secreto creado

Para ver el archivo secreto creado, lo primero que pense fue ir a ver el historial de la terminal, la misma se ecuentra en la carpeta `root` bajo el nombre de `.bash_history`, aca veremos todos los comandos que el atacante ejecuto en la terminal. En este archivo veremos la linea:

![insider4](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider4.png)

### Q5 - Programa usado por didyouthinkwedmakeiteasy.jpg

Nuevamente debemos buscar en el historial de la terminal para ver que programa ejecuto el archivo en cuestion

![insider5](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider5.png)

### Q6 - Checklist de Karen

Siguiendo la logica de creacion de archivos del atancante, me desplaze hasta la carpeta `Desktop`, en donde encontre la checklist de karen la misma conteniendo lo siguiente:

![insider6](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider6.png)

### Q7 - Cantidad de ejecuciones Apache

Volveremos a la carpeta `/var/log` e iremos al servicio en cuestion. Al ver los archivos vemos que estan todos vacios, particularmente veremos con atencion el archivo `access.log`, podemos inferir que al estar vacios los mismo este servicio no fue ejecutado

![insider7](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider7.png)

### Q8 - Prueba de ataque a otra maquina

Arranque mi busqueda desde la carpeta `root`, en donde me llamo la atencion un archivo en particular:

![insider8](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider8.png)

Veremos una imagen que nos da una prueba de que esta maquina se uso para atacar otra.

### Q9 - De quien se burlaba Karen?

La pregunta menciona que Karen se estaba burlando de alguien a traves de un script `bash`, autoamitcamente me diriji al historial de la terminal en donde encontre que en la carpeta `Docuements` se creo una carpeta de nombre `firsthack`. Una vez dentro de la carpeta en cuestion vermos varios archivos, el cual uno solo contiene la respuesta.

![insider9](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider9.png)

### Q10 - Usuario usando sudo

Para ver esto debemos ir nuevamente a la ruta `/var/log`, pero esta vez ver el `auth.log` buscando la hora en cuestion.

![insider10](../../assets/Cyberdefnders/Endpoint%20Forensics/Insider/insider10.png)

### Q11 - Directorio actual bash

Ya lo vimos anteriormente en el historial de la terminal, nos encontramos en la carpeta `firsthack`, dentro de `Documents`

Y con esto finalizamos el write up de este laboratorio. Espero que te haya servido.