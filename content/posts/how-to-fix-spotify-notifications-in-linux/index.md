---
title: 'How to Fix Spotify Notifications in Linux'
date: '2026-06-23T14:22:53-06:00'
tags: ["linux 🐧"]      # "linux 🐧"
              # "tecnología 💻"
              # "religión ✝️"
              # "personal"
              # "filosofía 👨‍👩‍👦"
              # "política 📜"
readingTime: true
toc: false
draft: false
image: '/posts/how-to-fix-spotify-notifications-in-linux/cover.png'
---

Somewhere near Feb. 2026 notifications when changing a song in Spotify stopped working for me in Artix Linux. I don't know if this is a general issue with Spotify, something specifically related to running it without systemd or simply something wrong with my system, but I created a workaround after looking through a couple of sources. Hopefully this can help you if you're having the same issue.

The workaround basically consists in locally overriding the Spotify .desktop file to launch Spotify with a separate workaround script that handles notifications.

## The workaround script

This is the script that handles notifications. Note that I've stored it under `$HOME/.local/bin/spotify-notify` and `$HOME/.local/bin` is part of my `$PATH`.

{{< github-raw
    url="https://raw.githubusercontent.com/danielml-mx/dotfiles/refs/heads/master/.local/bin/spotify-notify"
    lang="sh"
>}}

It basically starts Spotify with a playerctl instance which continuously reads for track changes within Spotify. Whenever one is registered, the track's art is extracted to `/tmp/spotify-song-art.jpg` and a notification is displayed with said art.

## The .desktop file

So that this script runs whenever and wherever Spotify is called, we must create a .desktop file that overrides the original. You can edit the original `spotify.desktop` file and redirect it to the workaround script or, even better, create a new entry under `$HOME/.local/share/applications/spotify.desktop`; here's mine:

{{< github-raw
    url="https://raw.githubusercontent.com/danielml-mx/dotfiles/refs/heads/master/.local/share/applications/spotify.desktop"
    lang="ini"
>}}

All I really did was change the `Exec` entry from `spotify` to `spotify-notify`, which itself launches Spotify. You might also want to consider creating an alias for your shell that does a similar mapping if you often launch Spotify from your terminal.

