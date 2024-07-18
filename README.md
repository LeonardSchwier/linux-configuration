# linux-configuration
A collection of commands and references to set up my Linux machine

## Installation process

- [ ] Create bootable usb-stick/ disk with https://etcher.balena.io/
- [ ] Walkthrough video: https://www.youtube.com/watch?v=gddlhr9ST9Y
- [ ] Use gdm3 as window manager: https://www.linuxfordevices.com/tutorials/linux/gdm3-vs-lightdm
- [ ] Force EFI (if required)
- [ ] Bugfix if debian always boots into tty https://www.reddit.com/r/debian/comments/15vh1le/debian_12_always_boots_into_tty/

## Add user to root group

> https://stackoverflow.com/questions/47806576/linux-username-is-not-in-the-sudoers-file-this-incident-will-be-reported

```
su  
nano /etc/sudoers
```

- [ ] add *myUsername* as it is written for *sudo*

## Set Monday as first day of the week

```
sudo nano /etc/default/locale

LANG="en_US.UTF-8" LC_TIME="en_GB.UTF-8" LC_PAPER="en_GB.UTF-8" LC_MEASUREMENT="en_GB.UTF-8"
```

> https://askubuntu.com/questions/6016/how-to-set-monday-as-the-first-day-of-the-week-in-gnome-2-calendar-applet


## Enable fingerprint authentication

```
sudo apt install fprintd libpam-fprintd 

fprintd-enroll -f right-middle-finger 

sudo pam-auth-update --enable fprintd
```

> https://forty.sh/posts/2021-02-19-enabling-fingerprint-authentication/

## Customize Gnome tweaks

- [ ] Tilebar: enable *minimise & maximise buttons*
- [ ] Touchpad: enable *tab to click*

## Install software stack

### Essential packages via apt


```
sudo apt install chromium thunderbird vlc wireguard audacity calibre baobab filezilla fuse nmap gimp libreoffice openvpn opensnitch yt-dlp gnome-shell-extension-appindicator borgbackup curl r-base-core evolution-ews plank rclone python3-pip python3-full ssh cmake rsync heif-gdk-pixbuf sshfs syncthing npm php php-dom php-mbstring fprintd libpam-fprintd
```
### Software via flatpak

```
sudo apt install flatpak gnome-software-plugin-flatpak

sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

flatpak install com.vscodium.codium com.bitwarden.desktop com.slack.Slack com.spotify.Client com.todoist.Todoist com.yubico.yubioath info.portfolio_performance.PortfolioPerformance net.ankiweb.Anki org.cryptomator.Cryptomator org.mozilla.firefox org.standardnotes.standardnotes org.zotero.Zotero md.obsidian.Obsidian org.signal.Signal io.github.shiftey.Desktop
```

### Other proprietary & 3rd party software

- [ ] https://posit.co/download/rstudio-desktop/

## Python, python libraries & OpenAI tools

> https://stackoverflow.com/questions/75608323/how-do-i-solve-error-externally-managed-environment-every-time-i-use-pip-3  
> https://platform.openai.com/docs/quickstart?context=python

```
sudo apt install python3-pip  

sudo mv /usr/lib/python3.11/EXTERNALLY-MANAGED /usr/lib/python3.11/EXTERNALLY-MANAGED.old

pip3 install openai pandas hashlib math time json os
```

## Install packages to be able to install tidyverse (R package)
```
sudo apt install libssl-dev libcurl4-openssl-dev unixodbc-dev libxml2-dev libmariadb-dev libfontconfig1-dev libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev
```

## yt-dlp via pip

> Install yt-dlp via pip not apt to get latest

- [ ] https://github.com/yt-dlp/yt-dlp/wiki/Installation

## Gnome extensions

- [ ] https://extensions.gnome.org/extension/1031/topicons/
- [ ] https://extensions.gnome.org/extension/615/appindicator-support/
- [ ] https://github.com/ubuntu/gnome-shell-extension-appindicator

## Local large-language-models (LLMs)

- [ ] https://ollama.ai/

## Set up backup with Vorta/ Borg base

- [ ] https://docs.hetzner.com/robot/storage-box/access/access-ssh-rsync-borg/
> https://github.com/borgbase/vorta/issues/732

```
borg init ssh://uXXXXX@uXXXXX.your-storagebox.de:23/./backups/thinkpad2024 --encryption=repokey

cat ~/.ssh/id_uXXXXX.pub | ssh -p23 uXXXXX@uXXXXX.your-storagebox.de install-ssh-key

ssh-copy-id -p 23 -s uXXXXX@uXXXXX.your-storagebox.de
```

## Set up Microsoft 365 e-mail

- [ ] https://uwaterloo.atlassian.net/wiki/spaces/ISTKB/pages/290362068/How+to+setup+Microsoft+365+in+Thunderbird
- [ ] https://wiki.gnome.org/Apps/Evolution/EWS/OAuth2
- [ ] https://wiki.hhu.de/pages/viewpage.action?pageId=33622182

## Randomise MAC addresses

- [ ] Open Network Manager and define for each WIFI, if you want to get a random MAC address

## Adjust look & feel of LibreOffice

- [ ] https://www.howtogeek.com/788591/how-to-make-libreoffice-look-like-microsoft-office/
- [ ] https://www.libreofficehelp.com/change-libreoffice-default-look-and-feel/

## Start command line interface (cli) from login screen

- Press `ctrl alt f3`

## Get an overview of all installed packages

- Start the pre-installed `synaptic package manger`

## Remove broken app extensions

> https://askubuntu.com/questions/289514/how-to-completely-remove-a-gnome-shell-extension

## Run OpenAI python library

> https://platform.openai.com/docs/quickstart?context=python

```
cd /home/leonard/
source openai-env/bin/activate`
```

## Pass arguments to a shell script (ex. yt-dlp)

```
#!/bin/bash

echo -n "Which YouTube URL do you want to download?
"
read filename

yt-dlp -o "~/Dropbox/YouTube/%(uploader_id)s/%(upload_date)s_%(title)s.%(ext)s" "$filename"

echo "I downloaded $filename"`
```

## Mount Google Drive with rclone
> https://rclone.org/drive/#making-your-own-client-id
```
rclone mount --daemon --vfs-cache-mode full googledrive:/ ./GoogleDrive/ --drive-export-formats link.html
rclone mount --daemon --vfs-cache-mode full dropbox:/ ./DropboxRClone/

#rclone bisync googledrive:/ /home/leonard/GoogleDriveSync/ --create-empty-src-dirs --compare size,modtime,checksum --slow-hash-sync-only --resilient -MvP --drive-skip-gdocs --fix-case --resync --dry-run

```

## Completely erase a hard drive

> https://www.howtogeek.com/788591/how-to-make-libreoffice-look-like-microsoft-office/

```
# List hard drives
fdisk -l
# Format a disk
sudo mkfs -t `ext4` /dev/sdb1
# Write random numbers to a disk
sudo dd if=/dev/urandom of=/dev/sdX bs=4096 status=progress
```

## Solve random issues with rclone

> https://github.com/rclone/rclone/issues/5708

## Some other scripts to solve issues

```
reboot from initramfs shell  
reboot -f
```

## Perform SSH scripts within Docker container

> https://forum.openmediavault.org/index.php?thread/52096-compose-plugin-console-connect/&postID=387038#post387038

```
docker exec paperless_webserver_1 "python3 manage.py createsuperuser"
```