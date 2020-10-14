# Redes1-Practica4_201610649
Práctica 4 redes1. Configuración con VTP y STP

## Manual de Configuración

### Topología
La topología implementada consiste en:

Tres host simulados
Un host virtualizado con sistema operativo Tiny Linux
Tres Switch capa tres
Un Router simulado con dos interfaces

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/Topologia_p4.png)

### Distribución de Redes

| VLAN   | Dirección de Red | Primera Dirección Asignable | Última Dirección Asignable | Gateway         | Dirección de Broadcast |
|--------|:----------------:|----------------------------:|---------------------------:|----------------:|-----------------------:|
|  10    |  192.168.19.0    | 192.168.19.1                | 192.168.19.254             |192.168.19.254   | 192.168.19.255         |
|  20    |  192.168.29.0    | 192.168.29.1                | 192.168.29.254             |192.168.29.254   | 192.168.29.255         |

### Configuración de Dispositivos
    
| Departamento  | Dispositivo | Dirección IP  | Máscara de Red | Gateway        |
| ------------- | ----------- | ------------- |--------------- |--------------- |
| VENTAS        | TinyLinux-1 | 192.168.19.10 | 255.255.255.0  | 192.168.19.254 |
| VENTAS        | PC1         | 192.168.19.15 | 255.255.255.0  | 192.168.19.254 |
| CONTABILIDAD  | PC2         | 192.168.29.10 | 255.255.255.0  | 192.168.29.254 |
| CONTABILIDAD  | PC3         | 192.168.29.15 | 255.255.255.0  | 192.168.29.254 |

### Configuración de hosts
La configuración de los host simulados en GNS3 se realiza siguiendo estos pasos:

1. Ejecutar el comando show ip para verificar la información de red de la computadora
2. Con el siguiente comando configurar la IP y la puerta de enlace:ip <IP> <MASCARA DE RED> <GATEWAY>
3. Guardar la configuración con el comando save
A continuación se muestra la configuración de los host utilizados para esta práctica.

#### PC2
![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/host/host1.PNG)

#### PC3
![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/host/host2.PNG)

#### PC4
3![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/host/host3.PNG)

#### PC1
El host "TinyLinux-1" es un dispositivo virtualizado con el sistema operativo Tiny Linux. La configuración de red se realiza con los siguientes pasos:

1. Ejecutar el comando `ifconfig` para ver la información de red

2. Ejecutar el comando `ipconfig <NAME INTERFACE> < IP> netmask <MASK> up`

    En donde:
    NAME INTERFACE: Es el nombre de la interfaz
    IP: Es la dirección IP a asignar -MASK: Es la máscara de red
    
3. Agregar la puerta de enlace con el comando route add `default gateway <GATEWAY>`

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/host/hostv.PNG)

### Creación de Vlans

#### VTP 
VTP es un protocolo de capa 2 que gestiona la adición, eliminación y cambio de nombre de las VLAN reduciendo la administración en una red. VTP garantiza que todos los switches del dominio VTP estén al tanto de todas las VLAN.
Un servidor VTP, puede crear, modificar y eliminar VLAN y especificar otros parámetros de configuración (como la versión VTP y el pruning VTP) para todo el dominio VTP. Los servidores VTP anuncian su configuración de VLAN a otros switches en el mismo dominio VTP y sincronizan su configuración VLAN con otros switches basados en anuncios recibidos a través de enlaces troncales. El servidor VTP es el modo predeterminado.
Los clientes VTP se comportan de la misma manera que los servidores VTP, pero no puede crear, cambiar o eliminar VLAN en un cliente VTP.

##### VTP Server 
Se configuró el SW1 como vt server con los siguientes comandos: 
1. `vlan database`
2. `vtp domain <DOMINIO>`
3. `vtp password <PASSWORD>`
4. `vtp v2-mode`
5. `vtp server`

##### VTP Cliente
Se configuraron los switches SW2 y SW3 como clientes vtp para que sean capaces de replicar las vlan creadas en SW1. Se realizó la configuración con los siguientes comandos:
1. `vlan database`
2. `vtp domain <DOMINIO>`
3. `vtp password <PASSWORD>`
4. `vtp v2-mode`
5. `vtp client

Nota: El dominio y el password son sensibles a mayúsculas, por lo tanto se debe tener cuidado de que coincidan o las configuraciones no se replicarán.

Para ver las configuraciones de VLANs se utiliza el comando `show vlan-switch`
A continuación se muestra la salida de este comando en cada uno de los switches de la topología.

###### SW1 (Server)
![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/vlans/sh-vlan-sw1.PNG)

###### SW2 (Client)
![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/vlans/sh-vlan-sw2.PNG)

###### SW3 (Client)
![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/vlans/sh-vlan-sw3.PNG)

###### Switch 1 y 2 
Para los switches proveídos por GNS3 que no son programables por consola, se configuraron de la siguiente manera: 

1. Hacer doble clic para mostrar las propiedades.
2. En la parte de configuración, asignar a los puertos conectados a otro switch el tipo **dot1q**
3. Asignar a los puertos conectados a dispositivos finales el tipo **access** y la VLAN correspondiente
4. Dar clic en aplicar y luego en aceptar

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/vlans/esw-config-3.PNG)

### Configuración de router
La configuración del switch simulados en GNS3 se realiza siguiendo estos pasos: 
1. Verificar el estado de las interfaces del router con el comando: `show ip int brief`
2. Indicar la interfaz a configurar con el comando `int f0/X.Y` en donde X es el número de la interfaz y Y es el número de VLAN para la cual se configura la subinterfaz
3. Ejecutar el siguiente commando para configurar el encapsulamiento `encapsulation dot1Q X` en donde X es el número de VLAN
4. Ejecutar el siguiente comando para configurar la IP de la interfaz: `ip address <IP> <MASCARA DE RED>`
5. Ejecutar `exit`
6. Salir de la configuración de la interfaz con el comando  `exit`

A continuación se muestra la configuración del router utilizado para esta práctica y la salida del comando `show ip int brief` al finalizar.

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/vlans/router.PNG)

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/vlans/router-2.PNG)

### Configuración de Port-Channel
Etherchannel es una tecnología de CISCO que nos permite realizar una agrupación lógica de varios enlaces físicos ethernet para que estos enlaces sean tratados como un único enlace, permitiendo sumar la velocidad de cada puerto y de esta forma lograr un enlace troncal de alta velocidad.

En cada extremo de los switches capa tres, ejecutar los siguientes comandos: 

1. Ejecutar `interface range fastethernet 0/1 – 4` para ingresar a la configuración de las interfaces
2. Ejecutar `channel-group <N> mode on` en donde N es un número entre 1 y 64
3. `exit`
4. Ingresar a la configuración del channel-group `interface port-channel <N>` 
5. Configurar el enlace de forma troncal `switchport mode trunk`
6. `exit`
7. Repetir en ambos extremos de la conexión para cada port-channel

A continuación se muestra la salida del comando `sh etherchnanel port-channel` para verificar los port channel.

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/port-channel/port-channel-1.PNG)

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/port-channel/port-channel-2.PNG)

### Pruebas
Para probar la comunicación entre los cuatro host se utilizaran paquetes ICMP por medio del comando [ping](https://linux.die.net/man/8/ping) con el objetivo de verificar la conectividad.
A continuación se muestra la respuesta de la red al hacer ping desde cada uno de los host a todos los demás.

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/host/ping2.PNG)

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/host/ping3.PNG)
### Captura de paquetes

GNS3 nos permite capturar paquetes ICMP para analizar la transferencia de paquetes y verificar la conectividad. Esto se realiza por medio de un software llamado [Wireshark](https://www.wireshark.org/).
El procedimiento para iniciar el monitoreo es el siguiente: 

1. Encender los dispositivos de la topología

2. Dar clic derecho sobre la interfaz que desea monitorear.

3. Iniciar wireshark en caso que no se haya iniciado automáticamente.

Luego podrá observar en wireshark los paquetes capturados con la información respectiva.


A continuación se muestra la captura de paquetes de la interfaz f0/0 del router en donde se capturaron los paquetes ICMP generados por el comando ping desde el host con IP 192.168.10.70 hacia el host con IP 192.168.10.5.

##### Echo Request 

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/capturas/request.PNG)

`0000   00 50 79 66 68 03 c2 01 08 ec 00 00 81 00 00 0a   .Pyfh...........
0010   08 00 45 00 00 54 49 af 00 00 ff 01 c9 96 c0 a8   ..E..TI.........
0020   13 fe c0 a8 13 14 00 00 78 bd af 49 00 05 08 09   ........x..I....
0030   0a 0b 0c 0d 0e 0f 10 11 12 13 14 15 16 17 18 19   ................
0040   1a 1b 1c 1d 1e 1f 20 21 22 23 24 25 26 27 28 29   ...... !"#$%&'()
0050   2a 2b 2c 2d 2e 2f 30 31 32 33 34 35 36 37 38 39   *+,-./0123456789
0060   3a 3b 3c 3d 3e 3f                                 :;<=>?`


##### Eco Reply

![image](https://github.com/EmmGarci4/Redes1-Practica4_201610649/blob/main/img/capturas/reply.PNG)

`0000   c2 01 08 ec 00 00 00 50 79 66 68 03 81 00 00 0a   .......Pyfh.....
0010   08 00 45 00 00 54 49 af 00 00 40 01 88 97 c0 a8   ..E..TI...@.....
0020   13 14 c0 a8 13 fe 08 00 70 bd af 49 00 05 08 09   ........p..I....
0030   0a 0b 0c 0d 0e 0f 10 11 12 13 14 15 16 17 18 19   ................
0040   1a 1b 1c 1d 1e 1f 20 21 22 23 24 25 26 27 28 29   ...... !"#$%&'()
0050   2a 2b 2c 2d 2e 2f 30 31 32 33 34 35 36 37 38 39   *+,-./0123456789
0060   3a 3b 3c 3d 3e 3f                                 :;<=>?`
