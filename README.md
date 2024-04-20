# ArchLInux

Esta es mi configuracion de Archlinux 
#Install
### Internet 
Primero debemos virificar el internet para eso usamos el comando ```iwtcl```  
https://h4ckseed.wordpress.com/2020/12/21/iwctl-en-archlinux/

### Estructura para una particion 
Los sistemas BIOS son los mas basicos normalmente se almacena en una memoria flash en la propia placa base e independiente del almacenamiento del sistema.

# Configuraciones adicionales 
### Add Virtual box 
Dependencias 
sudo pacman -S virtualbox-host-modules-arch

Instalar Virtual Box 
sudo pacman -S virtualbox

Add your user account to the “vboxusers” group
sudo usermod -aG vboxusers $USER
verificar con groups $USER

