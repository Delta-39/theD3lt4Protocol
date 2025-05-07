# :open_file_folder: Reveal Lab

Esta guia es una ayuda para la resolucion del laboratorio [Reveal](https://cyberdefenders.org/blueteam-ctf-challenges/ramnit/) de cyberdefenders.

## Escenario

Nuestro sistema de detección de intrusiones ha alertado sobre un comportamiento sospechoso en una estación de trabajo, lo que apunta a una probable intrusión de malware. Se ha realizado un volcado de memoria de este sistema para su análisis. Tu tarea es analizar este volcado, rastrear las acciones del malware y reportar los hallazgos clave.

### Q1 - Proceso responsable de la actividad maliciosa

Arranque por correr los plugins de **`volatility`** correspondientes a windows, pslist y malfind. En los mismos no encontre nada que me llamase la atención por lo que decidi correr sobre el volcado de memoria el plugin `cmdline`, este plugin me devuelve los ultimos comandos corrdidos sobre la consola `cmd`. En esta encontre un proceso que me parecio raro.

![ramnit1](../../assets/Cyberdefnders/Endpoint%20Forensics/Rammit%20Lab/rammit1.png)

La mayoria de los ejecutiables se suelen correr desde la carpeta `system32` cuando se trata de windows, me llamo la tencion que un ejecutable, y mas trantadose de chrome, se ejecute desde la carpeta de `downloads`.

### Q2 - Ruta del ejecutbale sospechoso

Como podemos ver en la captura anterior, aparece la ruta del ejecutable sospechoso.

### Q3 - Direccion IP con la que se comunica el malware.

Para esta pregunta corri el plugin de `volatility` llamado **`windows.netscan`**, este plugin lo que hace es darme el detalle de las conexiones de red activas. Encontre lo siguiente:

![ramnit2](../../assets/Cyberdefnders/Endpoint%20Forensics/Rammit%20Lab/ramnit2.png)

### Q4 - Ciudad asociada a la direccion IP

Aca lo que debemos hacer es simplemente irnos a la web de [What's my ip](https://whatismyipaddress.com/), colocar la direccion ip que encontramos en el punto anterior y nos dara la ciudad en cuestion.

![ramnit3](../../assets/Cyberdefnders/Endpoint%20Forensics/Rammit%20Lab/ramnit3.png)

### Q5 - Hash del archivo malicioso

En esta pregunta use el plugin **`windows.dumpfiles`** indicado el `PID` del archivo que queria obtener, ya con el ejecutable solamente hay que hacerle un hash **`SHA1`** y listo.

![ramnit4](../../assets/Cyberdefnders/Endpoint%20Forensics/Rammit%20Lab/ramnit4.png)

### Q6 - Timestamp de compilación

Ya que obtuvimos el hash en el punto anterior, ahora nos dirijiremos a la pagina de [VirusTotal](https://www.virustotal.com/), en donde colocaremos el hash obtenido. Una ves hecho lo anterior buscaremos en el apartado de detalles y encontraremos la fecha de compilación del archivo

![ramnit5](../../assets/Cyberdefnders/Endpoint%20Forensics/Rammit%20Lab/ramnit6.png)

### Q7 - Dominio con el que se comunica

Manteniendome en la pagina de **`VirusTotal`** iremos al apartado de relaciones en donde ecnotraremos los dominios y subdominions con los que este malware se intenta comunicar. Uno en particular es la respuesta correcta.

![ramnit6](../../assets/Cyberdefnders/Endpoint%20Forensics/Rammit%20Lab/ramnit5.png)


¡Y listo! Con esto terminamos esta guia, espero que te haya servido.