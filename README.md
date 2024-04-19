# ArchLInux

Esta es mi configuracion de Archlinux 
#Install
Primero debemos virificar el internet para eso usamos el comando ```iwtcl```


# Configuraciones adicionales 
### Add Virtual box 
Dependencias 
sudo pacman -S virtualbox-host-modules-arch

Instalar Virtual Box 
sudo pacman -S virtualbox

Add your user account to the “vboxusers” group
sudo usermod -aG vboxusers $USER
verificar con groups $USER

