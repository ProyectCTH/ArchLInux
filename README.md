[![archtv.png].

# ArchLinux
Hola, te presento la guia de instalacion de Archlinux. 


# Preinstalación


Lo que se busca aqui es tener las herramientas basicas para comenzar la instalacion, no todos los pasos son necesarios.

## Lest's go

- Imagen ISO: Debes conseguir una imagen ISO, lo recomendable es obtenerlo desde el enlace torrent pero descargalo de donde sea contal que lo verifiques esta bien. URL : https://archlinux.org/download/
  
- Verificar las firmas: Para verificar tu imagen solo coloca este comando, y compara la llave. 
  ```
  gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
  ```

- Preparar un medio de instalacion y arrancar un entorno live: Aqui tienes que preparar un dispositivo para la imagen ISO, vamos usar un USB. Para bootearlo puede usar rufus que es el mejor, pero tambien tienes otras opciones como Balena o Etcher. Un punto importante es que en la mayoria de PC con windows no arranca el UBS, la solucion a este problema es desabilitar el "modo seguro" desde la BIOS del sistema y nice.
  
## Terminal de la  ISO 
- Definir la configuracion de teclado: Para ver el listado de opciones que tienes solo coloca esto ```ls /usr/share/kbd/keymaps/**/*.map.gz``` , el comando para cambiar es ```loadkeys ``` , vamos a usar el latinoamericano ```loadkeys latam```

- Verificar la modalidad del arranque: Este paso es importante, porque dependera cuantas particiones vas hacer.
  ```
  # Para que sistema tiene coloca esto, si no aparece nada tienes un sistema BIOS en caso contrario tienes UEFI. 
  ls /sys/firmware/efi/efivars
  ```
- Conectarte a internet: En este paso usa el comando ```iwctl```. https://h4ckseed.wordpress.com/2020/12/21/iwctl-en-archlinux/ 
- Actualizar el reloj del sistema ( opcional ) ( innesario )
- Particion, formateo y montaje: Vamos realizar este paso para un sistema UEFI, para sistemas BIOS es muy similar pero sin los pasos para de la particion UEFI.  
### Estructura y montaje de las particiones 
Los sistemas BIOS son los mas basicos normalmente se almacena en una memoria flash en la propia placa base e independiente del almacenamiento del sistema.


# Install

En esta parte vamos a imprementar el sistema basico de Archlinux, adempas de descargar  paquetes y configurar nuestro sistema.
## Kernel basico y paquetes 
```
pacstrap -K /mnt base linux linux-firmware
o
pacstrap -K /mnt base base-devel linux linux-firmware networkmanager wpa_supplicant grub 
```
Lo que se esta instalando aqui es esto : 
- base linux linux firmware , son los paquetes del kernel y la base del SO linux
- base-devel,  es un grupo de paquetes que incluye herramientas necesarias para la construcción (compilación y vinculación)
- networkmanager, son paquetes para el internet
- wpa_supplicant, paquetes para el wifi
- grub, paquetes para el sistema de arraque uefi

### generar fstab 
```
genfstab -U /mnt >> /mnt/boot/efi
```

### Ingresar al sistema base 
Con Esto ya tenemos instalado nuestro sistema base de Archlinux en la carpeta /mnt, solo queda ingresar y hacer magia. 
```
arch-chroot /mnt
```
## Time 
Configuramos el reloj. 
```
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
// generate /etc/adjtime
hwclock --systohc
```
## Localization 
Configuramos nuestra localizacion. 
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
## Instalacion del grub
Primero se descarga el grub y despues lo instalamos , por ultimo lo configuramos 
```
pacman -S efibootmgr op-prober
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
### Configuracion del sudo 
Instalamos sudo.
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
Con esto lograste instalar Archlinux lo que viene despues es parte de tu imaginacion y tiempo de busqueda, suerte ya eres un linuxero novato lvl -0 


Me estaba olvidando inicia tu internet con  ```sudo systemctl start NetworkManager.service``` y para que lo tengas iniciado siempre con ```sudo systemctl start NetworkManager.service```



