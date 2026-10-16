+++
date = '2024-12-10T19:53:49-06:00'
draft = false
title = 'Instalación de Arch Linux'
toc = true
image = "arch-install.png"
tags = ["tecnología 💻"]
+++

A continuación presento una lista de los comandos utilizados en [mi video guía de instalación de Arch Linux](https://youtu.be/rDiilGzZNsE).

### ISO (si es que se está instalando desde una instalación pre-existente de Linux)
```
sudo dd if=arch-2024-12-04.iso of=/dev/sdb
```

Nótese que `if` significa "input file", la cual es el ISO descargado, mientras que `of` es "output file" y debe ser el dispositivo USB al cual se le va a quemar el ISO.



### TECLADO (disponibles en /usr/share/kbd/keymaps)
```
loadkeys la-latin1
```

### VERIFICAR MODO DE ARRANQUE (GPT/UEFI v MBR)
```
cat /sys/firmware/efi/fw_platform_size
```

### CONECTARSE A INTERNET (recomendable conectarse por ethernet)
```
ping danielml.mx # si se obtiene información, se está conectado a Internet
ip a # para obtener nombre de la estación
iwctl --passphrase passphrase station name connect SSID
```

### CREAR LAS PARTICIONES
```
cgdisk /dev/sdX
```

- Hacer partición boot
- Hacer partición swap (código 8200)
- Hacer partición root

### FORMATEAR LAS PARTICIONES
```
mkfs.vfat [boot]
mkfs.ext4 [/]
mkswap /dev/swap_partition
```

### MONTAR LAS PARTICIONES
 ```
cd /mnt
mount /dev/sdXN /mnt
mkdir boot
mount /dev/sdXN boot/
```

### SELECCIONAR MIRRORS (opcional, recomendado)
```
vi /etc/pacman.d/mirrorlist
```
Mover mirrors más cercanos geográficamente a la cima del archivo.

### INSTALACIÓN DE PAQUETES BASE
```
pacstrap /mnt base base-devel linux linux-firmware wpa_supplicant dhcpcd 
```

### GENERAR TABLA DE FS (FILE SYSTEM TABLE)
#### SI ESTÁS HACIENDO DUAL BOOT CON WINDOWS
```
mkdir boot/efi
mount /dev/sda1 boot/efi
```

```
genfstab -U /mnt / >> /mnt/etc/fstab
```

### ENTRAR AL SISTEMA
```
arch-chroot /mnt
```

### GENERAR LOCALES
```
nvim /etc/locale.gen   # descomentar los locales preferidos (ej. en_US.UTF8
locale-gen
nvim /etc/locale.conf  # escribir LANG=[locale preferido] (ej. LANG=en_US.UTF8)
```
 
### ELEGIR ZONA DE TIEMPO
```
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
```

### ELEGIR FORMATO DE TECLADO
```
nvim /etc/vconsole.conf     # escribir KEYMAP=[teclado preferido] (ej.KEYMAP=la-latin1)
```

### CREAR HOSTNAME
```
nvim /etc/hostname      # escribir nombre de máquina de tu elección
```


### ESTABLECER HOSTS
```
nvim /etc/hosts     # escribir lo siguiente, reemplazando [hostname] con el nombre de máquina seleccionado
# 127.0.0.1	localhost
# ::1		localhost
# 127.0.1.1	[hostname].locadomain	[hostname]
```

### COMPILAR MKINITCPIO
```
mkinitpcio -p linux
 ```

### INSTALAR BOOTLOADER
```
pacman -S grub os-prober efibootmgr
```

#### SI ESTÁS HACIENDO DUAL BOOT CON WINDOWS
```
nvim /etc/default/grub      # descomentar GRUB_DISABLE_OS_PROBER=false
```

```
grub-install --target=x86_64-efi --efi-directory=boot/efi/ --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

### CREAR CONTRASEÑA
```
passwd
```



