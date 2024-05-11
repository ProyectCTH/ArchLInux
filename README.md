# ArchLinux

Hola te presento una configuraciòn basica para comenzar en Archlinux siguiendo la guia de la Wiki 

# Preinstalación

Lo que se busca aqui es tener las herramientas basicas para comenzar la instalacion, no todos los pasos son necesarios.

### Lest's go

- Obtener la imagen ISO: Debes conseguir una imagen de la wiki, lo recomendable es obtenerlo desde el enlace torrent.
  https://archlinux.org/download/
  
- Verificar las firmas: Para verificar tu imagen solo coloca este comando, y compara la llave. 
  ```
  gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
  ```

- Preparar un medio de instalacion / arrancar un entorno live: Aqui tienes que preparar un dispositivo para la imagen ISO, vamos usar un USB. Para bootearlo puede usar rufus que es el mejor, pero tambien tienes otras opciones como Balena Etcher.
  
- Definiri la configuracion de teclado: Para ver el listado de opciones que tienes solo coloca esto ```ls /usr/share/kbd/keymaps/**/*.map.gz``` , el comando para cambiar es ```loadkeys ``` , vamos a usar el latinoamericano ```loadkeys latam```

- Verificar la modalidad del arranque: Este paso es importante, porque dependera cuantas particiones vas hacer.
  ```
  # Para que sistema tiene coloca esto, si no aparece nada tienes un sistema BIOS en caso contrario tienes UEFI. 
  ls /sys/firmware/efi/efivars
  ```

- Conectarte a internet: Para conectarte 
  ```
  ```
    
- Actualizar el reloj del sistema

- Particion, formateo y montaje

# Install

### Internet 

Primero debemos virificar el internet para eso usamos el comando ```iwtcl```  

https://h4ckseed.wordpress.com/2020/12/21/iwctl-en-archlinux/

### Estructura y montaje de las particiones 

Los sistemas BIOS son los mas basicos normalmente se almacena en una memoria flash en la propia placa base e independiente del almacenamiento del sistema.

### Kernel basico de linux

```
pacstrap -K /mnt base linux linux-firmware
o
pacstrap -K /mnt base base-devel linux linux-firmware networkmanager wpa_supplicant grub 
```

### generar fstab 
```
genfstab -U /mnt >> /mnt/boot/efi
```

### Ingresar al sistema base 
```
arch-chroot /mnt
```

## Time 
```
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
// generate /etc/adjtime
hwclock --systohc
```

## Localization 
```
locale-gen
// /etc/locale.conf
LANG=en_US.UTF-8
// /etc/vconsole.conf
KEYMAP=de-latin1
```

## Network Configuration 
ingresar al ```/etc/hostname``` y escribir: 
```
yourhostname 
# /etc/hosts
127.0.0.1 localhost
::1       localhost
127.0.0.1 yourhostname.localhost yourhostname
```

### Instalacion de repositorio paru 
Creamos una carpeta para nuestro repos ```mkdir -p Desktop/nameuser/repos```
```
git clone https://aur.archlinux.org/paru-bin.git
// ingresamos a /paru-bin/
makepkg -si
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
%wheel ALL=(ALL) ALL
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


##ESNABSHOP

Descargamos wget : ```sudo pacman -S wget```
### shell 
Instalamos ```sudo pacman -S zsh``` y modificamos como root ```sudo su``` ```usermod --shell /usr/bin/zsh username```
Aplicamos localectl set-xl1-keymap es
instalamos locate para ver archivos en el sistema : ```sudo pacman -S locate```
# Paquetes esenciales para iniciar qtile
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

# Configuraciones adicionales 
### AUR helper
Instalar un AUR helper, por ejemplo yay:
```
# Para todo el sistema0
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
### Descargar torrent 
```
sudo pacman -S qbittorrent
```
# Repositorios de BlackArch
Creamos una carpeta en ```mdkdir blackarch``` 
```
curl -o https://blackarch.org/strap.h
chmod +x strap.sh
sudo su
./strap.sh
```
Para ver todos las herramientas que se puede descargar  ```pacman -Sgg | grep blackarch```
