# **DHCP**

## **Primeros pasos**

Antes de empezar con lo que nos pide la práctica, deberemos preparar la máquina virtual que hará como servidor y la que hará de cliente.

La del servidor deberá tener un adaptador en NAT y otro en red interna con la opción de permitir máquinas virtuales. La máquina del cliente pondremos solo un adaptador en red interna.

## Instalar paquete isc-dhcp-server

Cuando tengamos ya un ubuntu server instalado, tendremos que usar el siquiente comando para instalar DHCP:

***$ sudo apt install isc-dhcp-server***

Con esto ya estaría DHCP instalado, ahora tenemos que configurarlo.

## Editar para una configuración básica /etc/dhcp/dhcpd.conf

Tendremos que acceder a un archivo con nano:

***$ sudo nano /etc/dhcp/dhcpd.conf***

Cuando hayamos accedido, tendremos que añadir varias líneas, son las siguientes:

**subnet 172.16.0.0 netmask 255.255.255.0 {**

**range 172.16.0.2 172.16.0.200;**

**option subnet-mask 255.255.255.0;**

**option broadcast-address 172.16.0.255;**

**option routers 172.16.0.1;**

**option domain-name-servers 8.8.8.8 8.8.4.4;**

**option domain-name "XoelServer";**

**}**