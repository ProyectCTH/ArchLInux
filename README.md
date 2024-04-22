# ArchLInux
Esta es mi configuracion de Archlinux 
# Install
### Internet 
Primero debemos virificar el internet para eso usamos el comando ```iwtcl```  
https://h4ckseed.wordpress.com/2020/12/21/iwctl-en-archlinux/

### Estructura y montaje de las particiones 
Los sistemas BIOS son los mas basicos normalmente se almacena en una memoria flash en la propia placa base e independiente del almacenamiento del sistema.
### Kernel basico de linux
```
pacstrap -K /mnt base linux linux-firmware
```
### generar fstab 
```
genfstab -U /mnt >> /mnt/boot/efi
```
### Ingresar al sistema base 
```
arch-chroot /mnt
```
## Instalacion del grub
Primero se descarga el grub y despues lo instalamos , por ultimo lo configuramos 
```
pacman -S grub efibootmgr op-prober
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Archlinux
# en el caso de que haya un error hacerlo por defecto con el siguiente comando
grub-install --target=x86_64-efi --efi-directory=/boot/efi --removable
# por ultimo configurar el PATH
grub-mkconfig -o /boot/grub/grub.cfg 
```
### Creamos usuario y le damos permisos
Colocamos el password para el root y creamos nuestro usario, dandonle permisos 
```
passwd
useradd -m username
passwd username
usermod -aG wheel,video,audio,storage username
```
Para tener privilegios de superusuario necesitamos sudo:
```
pacman -S sudo
```
Edita /etc/sudoers con nano o vim y descomenta la línea con "wheel":
```
## Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL) ALL
```
Ahora ya puedes reiniciar:
```
# Sal de la imagen ISO, desmóntala y extráela
exit
umount -R /mnt
reboot
```
# Post Install
Podemos ingresar al wifi por NetworkManager:
```
# Lista las redes disponibles
nmcli device wifi list
# Conéctate a tu red
nmcli device wifi connect TU_SSID password TU_CONTRASEÑA
```
## Paquetes esenciales para iniciar qtile
Primero instala el paquete para gestores de ventana
```
sudo pacman -S xorg
```
Despues instalamos qtile con algunas aplicaciones utiles 
```
sudo pacman -S lightdm lightdm-gtk-greeter qtile xterm code firefox 
```
Por ultimo iniciamos el lightdm 
```
sudo systemctl enable lightdm
reboot
```
Despues de esto ya tenemos instados un sistema listo para funcionar con el gestor de ventanas qitile 
# Configuracion qtile
Todas las configuraciones lo vamos hacer en .config/qtile/config.py
Cambiar el terminal por alacritty:
```
sudo pacman -S alacritty
#Configuramos en config.py
Key([mod], "Return", lazy.spawn("alacritty")),
```  
Agregamos el menú como dmenu o rofi:
``` 
sudo pacman -S rofi
#Configuramos en config.py
Key([mod], "m", lazy.spawn("rofi -show run")),
Key([mod, 'shift'], "m", lazy.spawn("rofi -show")),
```
Para cambiar el tema de rofi:
```
sudo pacman -S which
rofi-theme-selector
```
Agregamos el gestor de ventanas Thunar: 
```   
sudo pacman -S thunar
#Configuramos en config.py
Key([mod], "r", lazy.spawn("thunar")),
``` 
Agregamos el gestor de un gestor de brillo Redshift:
``` 
sudo pacman -S Redshift
Key([mod], "r", lazy.spawn("redshift -O 2400")),
Key([mod, "shift"], "r", lazy.spawn("redshift -x")),
```
Agregamos el Screenshot scrot:
``` 
sudo pacman -S scrot
Key([mod], "s", lazy.spawn("scrot")),
    Key([mod, "shift"], "s", lazy.spawn("scrot -s")),
```
Agregamos el audios :
``` 
sudo pacman -S pavucontrol pulseaudio

```
# Configuraciones adicionales 
### AUR helper
Instalar un AUR helper, por ejemplo yay:
```
# Para todo el sistema 
sudo pacman -S base-devel git
cd /opt/
sudo git clone https://aur.archlinux.org/yay-git.git
sudo chown -R username:username yay-git/
cd yay-git
makepkg -si
# Para solo el usario 
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

### Add Virtual box 
Pagina de referencia https://linuxiac.com/how-to-install-virtualbox-on-arch-linux/
Dependencias 
```
sudo pacman -S virtualbox-host-modules-arch
```
Instalar Virtual Box 
```
sudo pacman -S virtualbox
```
Add your user account to the “vboxusers” group
```
sudo usermod -aG vboxusers $USER
```
verificar con groups $USER
```
groups $USER
```

### Descargamos nvim o vim
```
sudo pacman -S neovim
```
