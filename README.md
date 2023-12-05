# **DHCP**

## **Primeros pasos**

Antes de empezar con lo que nos pide la práctica, deberemos preparar la máquina virtual que hará como servidor y la que hará de cliente.

La del servidor deberá tener un adaptador en NAT y otro en red interna con la opción de permitir máquinas virtuales. La máquina del cliente pondremos solo un adaptador en red interna.

## Instalar paquete isc-dhcp-server.

Cuando tengamos ya un ubuntu server instalado, tendremos que usar el siquiente comando para instalar DHCP:

***$ sudo apt install isc-dhcp-server***

Con esto ya estaría DHCP instalado, ahora tenemos que configurarlo.

## Editar para una configuración básica /etc/dhcp/dhcpd.conf.

Tendremos que acceder a un archivo con nano:

***$ sudo nano /etc/dhcp/dhcpd.conf***

Cuando hayamos accedido, tendremos que añadir varias líneas, son las siguientes:

**subnet 172.16.0.0 netmask 255.255.255.0 {**

**range 172.16.0.2 172.16.0.200;**

**option subnet-mask 255.255.255.0;**

**option broadcast-address 172.16.0.255;**

**option routers 172.16.0.1;**

**option domain-name-servers 8.8.8.8, 8.8.4.4;**

**option domain-name "XoelServer";**

**}**

## Editar para configurar la interfaz de red /etc/Default/isc-dchp-server.

Accedemos al archivo con nano:

***$ sudo nano /etc/default/isc-dhcp-server***

Y ahora tenemos que poner en el apartado de **"INTERFACESv4"** ponemos el adaptador de red **"enp0s8"**.

## Declarar una subnet 172.16.0.0/16

Para ello tenemos que acceder al archivo de netplan y modificarlo para declarar la subred, entramos al archivo con este comando:

***$ sudo nano /etc/netplan/00-installer-config.yaml***

Ahora tendremos que, en el apartado del adaptador de **"enp0s8"**, poner lo siguiente:

    enp0s8:
        dhcp4: no
        addresses: [172.16.0.1/16]
        nameservers:
            addresses:
                - 8.8.8.8
                - 8.8.4.4
    version: 2                

Cuando ya lo hayamos guardado, lo lanzaremos con el comando:

***$ sudo netplan apply***

## Arrancar servicio con systemctl.

Simplemente se utiliza el siguiente comando:

***$ sudo systemctl start isc-dhcp-server***

## Comprueba el servicio con "systemctl status".

Con el comando:

***$ sudo systemctl status isc-dhcp-server***

Si está todo correctamente, debería ponerte que está **activo** y **corriendo**, si no lo pone, no estará bien redactado alguno de los archivos.

## Prueba con el cliente que se le asigna un ip en el rango.

Ahora si entramos con el cliente con el servidor encendido y vamos a opciones de red, veremos que se nos asignará una IP dentro del rango que hayamos puesto. También se podrá ver con **ip a**.

## Declarar una asignación por mac fija a 172.16.0.5

Tendremos que volver a modificar el archivo **/etc/dhcp/dhcpd.conf** para ello. 

Lo que tendremos que hacer es añadir unas líneas adicionales a lo que teníamos escrito ya. Lo colocaremos debajo de las líneas anteriores.

    host cliente2 {
        hardware ethernet 42:68:cd:5e:e8:2d;
        fixed-address 172.16.0.5;
    }

## Prueba con otro cliente que se le asigna la IP fija.

Ahora con el cliente de donde cogimos la MAC, realizamos el **ip a** y veremos que la ip que tiene el equipo es la que hemos asignado en esas líenas del apartado anterior.

## Comprueba con wireshark los mensajes del protocolo

En el cliente instalamos **wireshark** con el comando:

***$ sudo apt-get install wireshark***

Cuando lo tengamos instalado, accederemos a el y haremos distintos ping en el servidor, cuando los hagamos veremos en el wireshark los mensajes captados producidos por el ping.


