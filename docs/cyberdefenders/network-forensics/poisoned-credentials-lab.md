# :detective: Poisoned Credentials Lab

Esta guia es una ayuda para la resolucion del laboratorio [Poisoned Credentials](https://cyberdefenders.org/blueteam-ctf-challenges/poisonedcredentials/) de cyberdefenders.

## Escenario

El equipo de seguridad de tu organización ha detectado un aumento en la actividad sospechosa en la red. Existe la preocupación de que puedan estar ocurriendo ataques de envenenamiento de LLMNR (Resolución de Nombres Multicast de Enlace Local) y NBT-NS (Servicio de Nombres NetBIOS) dentro de la red. Estos ataques son conocidos por explotar estos protocolos para interceptar el tráfico de red y potencialmente comprometer credenciales de usuarios. Tu tarea es investigar los registros de red y analizar el tráfico de red capturado.

### Q1 - Consulta mal escrita

Aplique el filtro en wireshark de `llmnr`, entonces solamente debemos buscar la direccion en cuestion y encontraremos la particularidad que esta mal escrito fileshare, `fileshaare`. Hay que prestar atención a las ayudas de wireshark, en este caso a mi me marca en amarillo este paquete en particular, debido a que el mismo programa reconoce que no hay una respuesta a esta consulta `dns`.

![poisoned1](../../assets/Cyberdefnders/Network%20Forensics/Poisoned%20Credentials/poisoned1.png)

??? note "Envenanmiento de LLMNR"
    El ataque de envenenamiento de LLMNR y NBT-NS aprovecha la resolución de nombres en redes locales. Cuando un dispositivo no puede encontrar una dirección IP para un nombre en su caché o DNS, utiliza LLMNR o NBT-NS para solicitar esa información a otros dispositivos de la red. Un atacante configura su máquina para responder falsamente a estas solicitudes, haciéndose pasar por el dispositivo buscado. Esto puede llevar a que los usuarios envíen sus credenciales al atacante, permitiendo su captura y potencial explotación.

### Q2 - Dirección IP de máquina maliciosa

Aca simplemente lo que hice fue seguir la conversacion que origino la anterior consulta llmnr `fileshaare`, entoces solamente deberiamos buscar la respuesta a dicha consulta y obtendremos la direccion ip de la maquina en cuestion.

![poisoned2](../../assets/Cyberdefnders/Network%20Forensics/Poisoned%20Credentials/poisoned2.png)

### Q3 - Seguna maquina afectada.

Aca simplemente hay que buscar intentos de logueos fallidos via trafico `smb2`, y daremos con la direccion **`192.168.232.176`**

### Q4 - Nombre de usuario comprometido

Ahora debemos filtrar por trafico `smb2` y por la direccion ip del atacante, y encontraremos el paquete con el nombre de usuario comprometido.

![poisoned3](../../assets/Cyberdefnders/Network%20Forensics/Poisoned%20Credentials/poisoned3.png)

### Q5 - Hostname maquina vulnerada

Para encontrar el nombre de host de la maquina, deberiamos hacer un seguimiento de conversacion de tcp de el protocolo `smb2`, alli encontraremos el hostname de la maquina vulnerada.

![poisoned4](../../assets/Cyberdefnders/Network%20Forensics/Poisoned%20Credentials/poisoned4.png)

