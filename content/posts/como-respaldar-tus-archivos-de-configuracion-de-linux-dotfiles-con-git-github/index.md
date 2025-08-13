---
title: 'Cómo respaldar tus archivos de configuración de Linux (dotfiles) con Git/Github'
date: '2025-08-12T21:44:30-06:00'
tags: [ "linux 🐧",  "tecnología 💻"]      # "linux 🐧"
              # "tecnología 💻"
              # "religión ✝️"
              # "personal"
              # "filosofía 👨‍👩‍👦"
              # "política 📜"
readingTime: false
toc: false
draft: true
image: 'git-gud.png'
---
Estos comandos son los que utilicé en mi video explicando
cómo respaldar archivos de configuración con Git. Los dejo
aquí para que sea más fácil copiar y pegarlos. El tutorial
en sí es el video.

## Comandos
```sh
### Inicializar y configurar repo
git init --bare ~/.config/.dotfile-manager
alias dots='/usr/bin/git --git-dir="$HOME/.config/.dotfile-manager" --work-tree="$HOME"' # añadir a RC
dots remote add origin <repository_URL> 
dots branch -M master
dots config status.showUntrackedFiles no

### Añadir archivos y submódulos
dots add <path>
dots submodule add <repository_URL> <path>

### Copiar repo y archivos a un nuevo sistema
git clone --bare <repository_URL> $HOME/.config/dotfile-manager
alias dots='/usr/bin/git --git-dir="$HOME/.config/dotfile-manager" --work-tree="$HOME"' # añadir a RC
dots checkout
dots config --local status.showUntrackedFiles no

## En zsh debes activar la siguiente opción en tu RC para
## que el alias autocomplete las ubicaciones de los archivos
setopt completealiases
```

## Tutorial
{{< youtube TtLc4tayPh4 >}}
