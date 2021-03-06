# Kali-Pi

Install and setup Kali Linux Raspberry Pi image using macOS.

### Install Kali Linux

Look at the partition table to identify SD card.

```
$ diskutil list
[...]
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.5 GB    disk2
   1:             Windows_FAT_32 PI                      64.0 MB    disk2s1
   2:                      Linux                         7.3 GB     disk2s2
[...]
```

Format SD card to FAT32.

```
$ sudo diskutil eraseDisk FAT32 KALI MBRFormat /dev/disk2
```

Transfer Kali Linux img file to SD card.

```
$ sudo diskutil unmountDisk /dev/disk2 # First unmount device.
$ sudo dd if=kali-2.1.2-rpi2.img of=/dev/disk2 bs=512k # See status with INFO signal (Ctrl+T).
```

### Setup Kali Linux

Change keyboard layout if needed.

```
$ dpkg-reconfigure keyboard-configuration
$ reboot
```

Change root passwort.

```
$ passwd
[...]
```

Change SSH host keys.

```
$ rm /etc/ssh/ssh_host_*
$ dpkg-reconfigure openssh-server
$ service ssh restart
```

***Wi-Fi***

Generate pre-shared key.

```
$ wpa_passphrase [SSID] | grep psk | tail -n1 >> /etc/network/interfaces
```

Add Wi-Fi settings to /etc/network/interfaces.

```
$ cat /etc/network/interfaces
[...]
auto wlan0
iface wlan0 inet dhcp
   wpa-ssid [SSID]
   wpa-psk [PSK] # The PSK generated above.
[...]
```

Reboot to apply changes.

```
$ reboot
```

***Vino (VNC Server)***

Install vino.

```
$ apt-get update
$ apt-get install vino
```

Disable encryption and confirmation prompt.

```
$ gsettings set org.gnome.Vino require-encryption false
$ gsettings set org.gnome.Vino prompt-enabled false
```

Create a desktop entry to let vino autostart (after login).

```
$ cd ~/.config && mkdir autostart && cd autostart && vim.tiny vino.desktop 
```

Add the following lines to vino.desktop.

```
[Desktop Entry]
Type=Application
Name=Vino
Exec=/usr/lib/vino/vino-server
NoDisplay=true
```

Manually start vino.

```
$ /usr/lib/vino/vino-server &
```

### Setup Environment

```
$ apt-get install vim
$ apt-get install zsh
$ apt-get install wget
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
$ git clone https://github.com/amix/vimrc.git ~/.vim_runtime && sh ~/.vim_runtime/install_awesome_vimrc.sh
$ [...]
```

### Install Development Tools

```
$ apt-get install gcc
$ apt-get install gdb
$ apt-get install lldb
$ [...]
```

### Install Reverse Engineering Tools

```
$ apt-get install radare2
$ [...]
```

### Install All

```
$ apt-get install kali-linux-full
```
