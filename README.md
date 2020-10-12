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
| VENTAS        | PC1         | 192.168.19.10 | 255.255.255.0  | 192.168.19.254 |
| VENTAS        | PC3         | 192.168.19.15 | 255.255.255.0  | 192.168.19.254 |
| CONTABILIDAD  | PC2         | 192.168.29.10 | 255.255.255.0  | 192.168.29.254 |
| CONTABILIDAD  | PC4         | 192.168.29.15 | 255.255.255.0  | 192.168.29.254 |

### Configuración de hosts
La configuración de los host simulados en GNS3 se realiza siguiendo estos pasos:

1. Ejecutar el comando show ip para verificar la información de red de la computadora
2. Con el siguiente comando configurar la IP y la puerta de enlace:ip <IP> <MASCARA DE RED> <GATEWAY>
3. Guardar la configuración con el comando save
A continuación se muestra la configuración de los host utilizados para esta práctica.

#### PC2
#### PC3
#### PC4
#### PC1
El host "PC1" es un dispositivo virtualizado con el sistema operativo Tiny Linux. La configuración de red se realiza con los siguientes pasos:

1. Ejecutar el comando `ifconfig` para ver la información de red

2. Ejecutar el comando `ipconfig <NAME INTERFACE> < IP> netmask <MASK> up`

    En donde:
    NAME INTERFACE: Es el nombre de la interfaz
    IP: Es la dirección IP a asignar -MASK: Es la máscara de red
    
3. Agregar la puerta de enlace con el comando route add `default gateway <GATEWAY>`

### Creación de Vlans

### Configuración de router
La configuración del switch simulados en GNS3 se realiza siguiendo estos pasos: 
1. Verificar el estado de las interfaces del router con el comando: `show ip int brief`
2. Indicar la interfaz a configurar con el comando `int f0/<x>`
3. Ejecutar el siguiente comando para configurar la IP de la interfaz: `ip address <IP> <MASCARA DE RED>`
4. Activar la interfaz con el comando `no shut`
5. Salir de la configuración de la interfaz con el comando  `exit`

A continuación se muestra la configuración del router utilizado para esta práctica.



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
