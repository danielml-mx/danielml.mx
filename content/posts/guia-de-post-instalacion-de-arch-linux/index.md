---
title: 'Guía de post-instalación de Arch Linux'
date: '2025-02-12T23:35:49-06:00'
tags: [ "linux 🐧",  "tecnología 💻"]     
              # "linux 🐧"
              # "tecnología 💻"
              # "religión ✝️"
              # "personal"
              # "filosofía 🤔"
              # "política 📜"
toc: true
draft: false
image: ''
---

Esta página es material complementario de [mi video explicando la post-instalación de Arch Linux](https://www.youtube.com/watch?v=ucm74RJzdnk). Para más información puedes consultar la [guía de recomendaciones generales](https://wiki.archlinux.org/title/General_recommendations) de Arch Linux.

### Crear un nuevo usuario
- `useradd -m -G wheel <usuario>`
- `passwd <usuario>`

#### Establecer privilegios sudo
- `visudo`

#### Entrar como usuario

### Conectarse a internet

#### Generar estanza de red
- `wpa_passphrase "Mi Red" "m¡-c0ntr45eña"`
- `wpa_passphrase "Mi Red" "m¡-c0ntr45eña" >> /etc/wpa-supplicant/wpa-supplicant.conf`

#### Habilitar e iniciar servicios
- `sudo systemctl enable wpa_supplicant.service dhcpcd.service`
- `sudo systemctl start wpa_supplicant.service dhcpcd.service`

### Instalar entorno gráfico

#### Toma una decisión...
- Entorno de escritorio
    - KDE
    - xfce
- Manejador de ventanas (Xorg) / Compositor (Wayland)
    - dwm
    - i3
    - hyprland
    - sway

### Instalar paquetes básicos
- Audio
    - PipeWire
    - PulseAudio
- Gráficos
    - Nvidia...
- Fuentes de texto
    - `sudo pacman -S ttf-liberation`
    - `sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji`
- Página de manuales
    - `sudo pacman -S man`
- Navegador web
    - Firefox
- Explorador de archivos
    - Dolphin
- Terminal
    - alacritty
- Editor de texto
    - neovim

#### Comando corrido
`sudo pacman -S neovim alacritty dolphin firefox man ttf-liberation noto-fonts-cjk noto-fonts-emoji pipewire pipewire-pulse wireplumber git`

### Repositorio de usuarios
- yay
    - `git clone https://aur.archlinux.org/yay-git.git && cd yay-git/`
    - `makepkg -si`




