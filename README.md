# Ubuntu20 installation guide

Manual installation guide for fresh VPS based on Ubuntu 20.04.3 LTS with SSH.

## Root user

Change password

```
passwd
```

## Create user

Add new user 

```
$ adduser <user>
```

It's possibly installed but if not then

```
$ apt-get update
$ apt-get install sudo
```

Make it sudo-user 

```
$ usermod -a -G sudo <user>
```

We can use our own user since now.

## Emacs

Install text editor using

```
$ sudo apt-get install emacs
```

Configuration file `~/.emacs` to keep temporary saves in one place

```
(custom-set-variables
  ;; custom-set-variables was added by Custom.
  ;; If you edit it by hand, you could mess it up, so be careful.
  ;; Your init file should contain only one such instance.
  ;; If there is more than one, they won't work right.
 '(inhibit-startup-screen t))
(custom-set-faces
  ;; custom-set-faces was added by Custom.
  ;; If you edit it by hand, you could mess it up, so be careful.
  ;; Your init file should contain only one such instance.
  ;; If there is more than one, they won't work right.
 )

(setq backup-directory-alist `(("." . "~/.emacs.d/emacs-saves")))

(setq backup-by-copying t)

(setq delete-old-versions t
  kept-new-versions 6
  kept-old-versions 2
  version-control t)
```

## Bash prompt

Install required packages

```
$ sudo apt-get install ruby git
```

File `~/.bash_profile`

```
# set git branch in command prompt
export PS1="\[\033[38m\]\u@\h\[\033[01;34m\] \w \[\033[31m\]\`ruby -e \"print (%x{git branch 2> /dev/null}.lines.grep(/^\*/).first || '').gsub(/^\* (.+)$/, '(\1) ')\"\`\[\033[37m\]$\[\033[00m\] "
```

## SSH

Add your public key to `~/.ssh/authorized_keys`.

## Git

User

```
$ git config --global user.name "Jan Nowak"
$ git config --global user.email jannowak@example.com
```

Editor

```
$ git config --global core.editor emacs
```

Config

```
$ git config --list
```

# Hosting

## Language packs

Install required language packs

```
$ sudo apt-get -y install language-pack-pl
$ sudo apt-get -y install language-pack-de
$ sudo apt-get -y install language-pack-nb
```

## MariaDB

Installation

```
$ sudo apt-get install -q -y mariadb-server mariadb-client
```

Loading timezones

```
$ mysql_tzinfo_to_sql /usr/share/zoneinfo | sudo mysql -u root mysql
```

## PHP & NGINX

### NGINX

Installation

```
$ sudo apt update
$ sudo apt install nginx
```

Unlock firewall

```
$ sudo ufw app list
```

### PHP 8.0

Enabling PHP repository

```
$ sudo apt install software-properties-common
$ sudo add-apt-repository ppa:ondrej/php
```

Installation PHP 8.0

```
$ sudo apt update
$ sudo apt install php8.0-fpm
```

Check status

```
$ sudo systemctl status php8.0-fpm
```

Edit NGINX config file

```
server {

    # . . . other code

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.0-fpm.sock;
    }
}
```

Restart NGINX

```
$ sudo systemctl restart nginx
```

PHP extensions

```
$ sudo apt install php8.0-mysql php8.0-gd php8.0-mbstring php8.0-curl libphp-adodb
```

## Supervisor

Installation

```
$ sudo apt install supervisor
```