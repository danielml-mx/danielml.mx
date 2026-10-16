---
title: 'Herramientas y programas que uso'
date: '2026-10-08'
tags: ["tecnología 💻",
       "favoritos ⭐"]
toc: false
draft: false
image: 'screenshot-020225-111003.png'
---

Dejo aquí una lista de lo que uso cotidianamente. En general
priorizo programas libres, ligeros, rápidos y estéticos que
sigan la [filosofía de
UNIX](https://en.wikipedia.org/wiki/Unix_philosophy),
mientras que para *hardware* busco que puedan cumplir con
varios propósitos. A menos que me sea imposible, evito
software y hardware restrictivo como las Macs, Windows,
Chrome, Adobe Reader, etc.

Todos estos programas se encuentra o en los repositorios
oficiales de Arch o en el repositorio de usuarios (AUR).
Además, puedes consultar los archivos de configuración que
uso con muchos estos programas en el [repositorio de mis
archivos de
configuración](https://github.com/danielml-mx/dotfiles).

## Hardware
- **Computadora:** ThinkPad L13 Yoga Gen 2 con un procesador 11th Gen Intel i5-1145G7 (8) @ 4.400GHz
    - Esta laptop es lo más cercano que he encontrado a una laptop perfecta. Incluye un trackpoint, una pantalla táctil con pluma y un factor de forma ligero, aunque desearía que fuera tan sólo un poco más pequeña la relación de aspecto, algo así como la X200 clásica, y además como muchas laptops modernas las opciones de mejora son limitadas.
    - La pantalla táctil la verdad deja mucho que desear. No
      es tan amigable de usar como esperaba, y tiene varios
      problemas al detectar la pluma, pero basta para tomar
      unas cuantas notas.

## Software
- **Sistema operativo:** [Artix Linux](https://artixlinux.org)
    - Arch Linux sin SystemD. Es básicamente lo mismo pero
      mejor.
- **Manejador de ventanas:** [dwm](https://dwm.suckless.org)
    - [Fork personal](https://github.com/danielml-mx/dwm)
    - Hace mucho tiempo utilizaba KDE y me pareció el mejor entorno de escritorio de las opciones disponibles. También he intentado Wayland, particularmente hyprland, pero no me parece que esté suficientemente desarrollado todavía. 
    - Es 2026 y sigo utilizando dwm. He experimentado con [mangowm](https://mangowm.github.io/docs/), el cual es como un sucesor espiritual a dwm en Wayland, y aunque probablemente termine transicionando a él, dwm sigue siendo demasiado bueno.
- **Barra de estatus:** la predeterminada de dwm (con parches) + [dwmblocks-async](https://github.com/UtkarshVerma/dwmblocks-async)
- **Compositor:** [picom](https://github.com/yshui/picom)
- **Terminal:** [st](https://st.suckless.org/) con [tabbed](https://github.com/danielml-mx/tabbed)
- **Shell:** zsh
    - No utilizo ohmyzsh, sino he instalado los plugins de autocomplete, autosuggestions y syntax-highlighting.
    - Para el prompt personalizado, utilizo [p10k](https://github.com/romkatv/powerlevel10k) instalado manualmente.

<div style="display: block; margin-left: auto; margin-right: auto; text-align: center; font-size: 1.4rem; font-family: var(--article-font-family); width: 90%;">
  <img src="env.gif" style="border-radius: 10px"/>
  <figcaption>Breve demostración del software mencionado hasta ahora</figcaption>
</div>

- **Audio:** [PipeWire](https://wiki.archlinux.org/title/PipeWire)
- **Daemon de notificaciones:** dunst
- **Editor de texto:** [neovim](https://neovim.io/)
    - Finalmente me atreví a dejar el Vim original atrás al aprovechar todas las funcionalidades que ofrece utilizar una configuración basada en Lua. Además me ha permitido tener más y mejores plugins. Estoy satisfecho hasta ahora.
        - Autocompletado: [hrsh7th/nvim-cmp](https://github.com/hrsh7th/nvim-cmp)
        - Colores: [folke/tokyonight.nvim](https://github.com/folke/tokyonight.nvim)
        - Status-line: [nvim-lualine/lualine.nvim](https://github.com/nvim-lualine/lualine.nvim)

<div style="display: block; margin-left: auto; margin-right: auto; text-align: center; font-size: 1.4rem; font-family: var(--article-font-family); width: 70%;">
  <img src="screenshot-100826-163223.png" style="border-radius: 10px"/>
  <figcaption>La apariencia de mi Neovim actual, un WIP.</figcaption>
</div>

- **Navegador de archivos:** [lf](https://github.com/gokcehan/lf) con [ueberzugpp](https://github.com/jstkdng/ueberzugpp)
- **Navegador web:** [LibreWolf](https://librewolf.net/)
    - Fork de Firefox orientada a la privacidad y seguridad, aunque debo admitir que algunas de las funcionalidades que esperas de la web moderna no siempre funcionan con LibreWolf.
- **Visualizador de documentos:** [sioyek](https://github.com/ahrm/sioyek)
- **Visualizador de imágenes:** sxiv
- **Reproductor de multimedia:** mpv

