# Escalamiento de CMS usando un cluster docker

Este repositorio tiene las configuraciones usadas para poder simular un ambiente
clusterizado de docker basado en rancher, usando PCs de una sala de PC
peteneciente a la Facultad de Informática de la UNLP. 
La simulación utilizará rancheros booteado desde la red usando:

* [DHCP](https://www.isc.org/downloads/dhcp/)
* [TFTP](http://chschneider.eu/linux/server/tftpd-hpa.shtml)
* [iPXE](http://ipxe.org/)

## Inicio rápido para ansiosos

* En esta carpeta, conectar la placa ethernet a su PC
* Verificar si el `Vagrantfile` utiliza el nombre de la placa de su PC
  * Si así no fuera, cambiarlo. Podríamos plantear usar una variable de ambiente
* Correr `vagrant up`

### Comandos vagrant esenciales

* `vagrant up`: inicia la vm
* `vagrant destroy`: destruye la vm
* `vagrant provision`: reconfigura la vm

## El servidor de DHCP+TFTP

El servidor de DHCP configura un rango de IPs de la sala asignándoles a las PC
no solo su configuración IP, sino que además el servidor de TFTP.
Este servidor de TFTP también se aloja en la misma virtual que sirve DHCP con el
fin de proveer no un kernel sino una nueva ROM de iPXE encadenándo el booteo de
forma de poder realizar más adelante un booteo por HTTP.

### Por qué iPXE

PXE permite bootear máquinas usando TFTP e impone diversas limitaciones que iPXE
extiende. De esta forma podemos configurar nuestras máquinas descargando el
kernel usando HTTP

## Rancher OS

[Rancher OS](http://rancher.com/rancher-os/) es un Sistema Operativo muy pequeño que permite correr docker rápidamente,
permitiendo configurar la instalación usando [cloud init](https://cloud-init.io/)

## Los problemas

Al parecer Rancher OS booteado desde la RED no toma algunas opciones de cloud
init que se aplicarían en un sistema ya instalado. Estamos trabajando en
comprender como lograr configirar en RAM el estado inicial de las máquinas que
inician desde la red Rancher OS, a decir:

* Montar un disco local (si fuera necesario)
* Correr un contenedor luego de bootear. Por ejemplo para registrarse a un
  servidor de rancher

