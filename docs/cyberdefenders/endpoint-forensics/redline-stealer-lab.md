# :open_file_folder: Redline Lab

Esta guia es una ayuda para la resolucion del laboratorio [Red Line lab](https://cyberdefenders.org/blueteam-ctf-challenges/redline/) de cyberdefenders.

## Escenario

Como miembro del equipo de Seguridad Blue Team, tu asignación es analizar un volcado de memoria utilizando las herramientas Redline y Volatility. Tu objetivo es rastrear los pasos realizados por el atacante en la máquina comprometida y determinar cómo lograron evadir el Sistema de Detección de Intrusiones de Red (NIDS). Tu investigación implicará identificar la familia específica de malware utilizada en el ataque, junto con sus características. Además, tu tarea será identificar y mitigar cualquier rastro o huella dejada por el atacante.

### Q1 - Nombre del proceso sospechoso.

Inicie primeramente ejecutando el plugin de **`volatility3`** para verificiar que tipo de os comprendia este volcado de memoria, `windows.pslist`, ademas de listarme todos los procesos , si no es un volcado de memoria de linux nos va a devolver un erro con este plugin.

Ya como sabemos que es un volcado de memoria de windows, procedi a usar el plugin `windows.malfind`, el cual se centra en identificar procesos o áreas de memoria que podrían haber sido alterados por un atacante o utilizados para ejecutar código malicioso.

![redline1](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline1.png)

??? note "MZ en contexto de IR"
    El encabezado MZ es la firma de un archivo ejecutable en sistemas Windows. Su detección por windows.malfind es un indicio claro de que hay un ejecutable (potencialmente malicioso) cargado en la memoria, lo que apunta a posibles técnicas de inyección de código o malware en ejecución.

### Q2 - Nombre del proceso hijo

Para obtener esto aplique el plugin `windows.pstree`, el cual me da todo el "arbol" de procesos de este volcado de memoria, aca podremos identificar los procesos padres seguidos de los procesos hijos.

![redline2](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline2.png)

### Q3 - Proteccion de memoria

Esto ya lo vimos anteriormente cuando aplicamos el plugin **`windows.malfind`**, y obtuvimos los procesos malidiciosos, tambien nos da la proteccion de memoria del proceso en cuestion. Esta proteccion de memoria no son mas que los permisos que tiene esta pagina.

![redline3](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline3.png)

### Q4 - Proceso responsable de la VPN

Primero use el plugin de **`windows.netscan`** el cual me da todas las conexiones que tuvo este volcado de memoria, el cual me parecio raro el siguiente proceso:

![redline4](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline4.png)

Por lo que decedi investigarlo, termine encontrando que es un proceso hijo de otro que esta vinculado con conexiones vpn

![redline5](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline5.png)

A continuacion investigue el numero de proceso que tiene, y busque con el plugin **`windows.pstree`** para ver si tenia algun proceso padre que desencadene este.

![redline6](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline6.png)

### Q5 - Direccion ip atacante

Volveremos a emplementar nuevamente el plugin **`windows.netscan`**, para ver a direccion ip esta llamando el proceso malicioso que encontramos al principio , veremos la ip del atacante.

![redline7](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline7.png)

### Q6 - Famila de malware

Para esto tendremos que hacer un poquito de investigación, como primer medida fui directo a Virustotal y coloque la direccion ip del atacante a ver si me devolvia algo interesante

![redline8](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline8.png)

### Q7 - URL completa visitada

Para averiguar la url visitada me parecio correcto hacer uso del comando **`strings`**, el cual se utiliza para extraer cadenas de texto legibles (ASCII o Unicode) de archivos binarios o volcados de memoria. Yo lo que hice fue, usando **`grep`**, filtre las cadenas con la direccion ip del atacante

![redline9](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline9.png)

### Q8 - Direccion de destino

Usaremos el plugin **`windows.filescan`**,se utiliza para realizar un escaneo de las estructuras relacionadas con archivos abiertos en la memoria volátil. De aca usaremos nuevamente el comando **`grep`** para buscar por el nombre del proceso malicioso.

![redline10](../../assets/Cyberdefnders/Endpoint%20Forensics/redline%20stealer/redline10.png)


