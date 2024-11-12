### 🌞 Trouver le chemin vers le répertoire personnel de votre utilisateur

```powershell
me@Debian11:~$ cd /home/me
```

### 🌞 Vérifier les permissions du répertoire personnel de votre utilisateurs


``` powershell 
me@Debian11:~$ ls -l /home/me
total 36
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Desktop
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Documents
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Downloads
drwxr-xr-x 3 me me 4096 Nov 11 23:43 gameshell
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Music
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Pictures
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Public
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Templates
drwxr-xr-x 2 me me 4096 Nov  6 18:18 Videos
```

### 🌞 Trouver le chemin du fichier de configuration du serveur SSH

``` powershell
root@Debian11:~# find / -name "ssh_config"
/etc/ssh/ssh_config
```

### 🌞 Créer un nouvel utilisateur
```powershell
root@Debian11:/# sudo useradd -m -d /home/papie_alu marmotte
root@Debian11:/# sudo passwd marmotte
New password:
Retype new password:
passwd: password updated successfully
```
### 🌞 Prouver que cet utilisateur a été créé
```powershell
root@Debian11:/# cat /etc/passwd | grep marmotte
marmotte:x:1001:1001::/home/papie_alu:/bin/sh
```

### 🌞 Déterminer le hash du password de l'utilisateur marmotte
```powershell
root@Debian11:/# cat /etc/shadow | grep marmotte
marmotte:$y$j9T$cWon50qZ9YYW6DZUDU.yi/$0Iskt77VtRFRDTSi8CSY1tuhKMJg.BHZX8MvGMPFVd3:20039:0:99999:7:::
```

### 🌞 Tapez une commande pour vous déconnecter : fermer votre session utilisateur
```powershell
me@Debian11:~$ exit
logout
Connection to 192.168.13.100 closed.
```


### 🌞 Assurez-vous que vous pouvez vous connecter en tant que l'utilisateur marmotte

```powershell
PS C:\Users\philipon gouin matys> ssh marmotte@192.168.13.100
marmotte@192.168.13.100's password:
Linux Debian11 5.10.0-33-amd64 #1 SMP Debian 5.10.226-1 (2024-10-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Nov 12 03:36:20 2024 from 192.168.13.2
$ cd /
$ ls
bin  boot  dev  etc  home  initrd.img  initrd.img.old  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz  vmlinuz.old
$ ls /home/me       
Desktop  Documents  Downloads  gameshell  Music  Pictures  Public  Templates  Videos
```

### 🌞 Lancer un processus sleep
```powershell
me@Debian11:~$ ps aux | grep sleep
me         47972  0.0  0.0   5364   560 pts/1    S+   03:54   0:00 sleep 1000
me         47975  0.0  0.0   6240   708 pts/2    S+   03:54   0:00 grep sleep
```

### 🌞 Terminez le processus sleep depuis le deuxième terminal
```powershell
me@Debian11:~$ kill 47972
me@Debian11:~$ ps aux | grep sleep
me         47979  0.0  0.0   6240   640 pts/2    S+   03:55   0:00 grep sleep
```

### 🌞 Visualisez la commande en tâche de fond
```powershell
me@Debian11:~$ jobs -l
[1]+ 47981 Running                 sleep 10000 &
```

### 🌞 Trouver le chemin où est stocké le programme sleep

```powershell
root@Debian11:~# sudo find / -name sleep
/usr/bin/sleep
/usr/lib/klibc/bin/sleep
```
### 🌞 Tant qu'on est à chercher des chemins : trouver les chemins vers tous les fichiers qui s'appellent .bashrc
```powershell
root@Debian11:~# sudo find / -name .bashrc
/etc/skel/.bashrc
/root/.bashrc
/home/marmotte/.bashrc
/home/papie_alu/.bashrc
/home/me/.bashrc
find: ‘/run/user/1000/doc’: Permission denied
```

### 🌞 Vérifier que
```powershell
root@Debian11:~# which sleep
/usr/bin/sleep
root@Debian11:~# which ssh
/usr/bin/ssh
root@Debian11:~# which ping
/usr/bin/ping
root@Debian11:~# echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

### 🌞 Installer le paquet firefox>
```powershell
root@Debian11:~# apt install firefox
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Package firefox is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Package 'firefox' has no installation candidate
```

### 🌞 Utiliser une commande pour lancer Firefox

```powershell
root@Debian11:~# which firefox
/usr/bin/firefox
```

### 🌞 Mais aussi déterminer...
```powershell
root@Debian11:~# ls /etc/apt
apt.conf.d  auth.conf.d  listchanges.conf  listchanges.conf.d  preferences.d  sources.list  sources.list~  sources.list.d  trusted.gpg.d
root@Debian11:/etc/apt# nano sources.list
```
```
deb http://deb.debian.org/debian/ bullseye main
deb-src http://deb.debian.org/debian/ bullseye main

deb http://security.debian.org/debian-security bullseye-security main
deb-src http://security.debian.org/debian-security bullseye-security main

# bullseye-updates, to get updates before a point release is made;
# see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backports
deb http://deb.debian.org/debian/ bullseye-updates main
deb-src http://deb.debian.org/debian/ bullseye-updates main
```

