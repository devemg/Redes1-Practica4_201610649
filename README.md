# Redes1-Practica4_201610649
Práctica 4 redes1. Configuración con VTP y STP

## Manual de Configuración

### Topología
La topología implementada consiste en:

Tres host simulados
Un host virtualizado con sistema operativo Tiny Linux
Tres Switch capa tres
Un Router simulado con dos interfaces

------------------------------------------------FOTO---------------------------------------------------------------------------

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
------------------------------------------------FOTO---------------------------------------------------------------------------

#### PC3
------------------------------------------------FOTO---------------------------------------------------------------------------

#### PC4
------------------------------------------------FOTO---------------------------------------------------------------------------

#### PC1
El host "TinyLinux-1" es un dispositivo virtualizado con el sistema operativo Tiny Linux. La configuración de red se realiza con los siguientes pasos:

1. Ejecutar el comando `ifconfig` para ver la información de red

2. Ejecutar el comando `ipconfig <NAME INTERFACE> < IP> netmask <MASK> up`

    En donde:
    NAME INTERFACE: Es el nombre de la interfaz
    IP: Es la dirección IP a asignar -MASK: Es la máscara de red
    
3. Agregar la puerta de enlace con el comando route add `default gateway <GATEWAY>`

------------------------------------------------FOTO---------------------------------------------------------------------------


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
------------------------------------------------FOTO---------------------------------------------------------------------------

###### SW2 (Client)
------------------------------------------------FOTO---------------------------------------------------------------------------

###### SW3 (Client)
------------------------------------------------FOTO---------------------------------------------------------------------------

###### Switch 1 y 2 
Para los switches proveídos por GNS3 que no son programables por consola, se configuraron de la siguiente manera: 

1. Hacer doble clic para mostrar las propiedades.
2. En la parte de configuración, asignar a los puertos conectados a otro switch el tipo **dot1q**
3. Asignar a los puertos conectados a dispositivos finales el tipo **access** y la VLAN correspondiente
4. Dar clic en aplicar y luego en aceptar


### Configuración de router
La configuración del switch simulados en GNS3 se realiza siguiendo estos pasos: 
1. Verificar el estado de las interfaces del router con el comando: `show ip int brief`
2. Indicar la interfaz a configurar con el comando `int f0/X.Y` en donde X es el número de la interfaz y Y es el número de VLAN para la cual se configura la subinterfaz
3. Ejecutar el siguiente commando para configurar el encapsulamiento `encapsulation dot1Q X` en donde X es el número de VLAN
4. Ejecutar el siguiente comando para configurar la IP de la interfaz: `ip address <IP> <MASCARA DE RED>`
5. Ejecutar `exit`
6. Salir de la configuración de la interfaz con el comando  `exit`

A continuación se muestra la configuración del router utilizado para esta práctica y la salida del comando `show ip int brief` al finalizar.

------------------------------------------------FOTO---------------------------------------------------------------------------

------------------------------------------------FOTO---------------------------------------------------------------------------



### Pruebas
Para probar la comunicación entre los cuatro host se utilizaran paquetes ICMP por medio del comando [ping](https://linux.die.net/man/8/ping) con el objetivo de verificar la conectividad.
A continuación se muestra la respuesta de la red al hacer ping desde cada uno de los host a todos los demás.


### Captura de paquetes

GNS3 nos permite capturar paquetes ICMP para analizar la transferencia de paquetes y verificar la conectividad. Esto se realiza por medio de un software llamado [Wireshark](https://www.wireshark.org/).
El procedimiento para iniciar el monitoreo es el siguiente: 

1. Encender los dispositivos de la topología

2. Dar clic derecho sobre la interfaz que desea monitorear.

3. Iniciar wireshark en caso que no se haya iniciado automáticamente.

Luego podrá observar en wireshark los paquetes capturados con la información respectiva.


A continuación se muestra la captura de paquetes de la interfaz f0/0 del router en donde se capturaron los paquetes ICMP generados por el comando ping desde el host con IP 192.168.10.70 hacia el host con IP 192.168.10.5.

##### Echo Request 
------IMAGEN--------

------CODIGO------                                         >?


##### Eco Reply

------IMAGEN--------

------CODIGO------
