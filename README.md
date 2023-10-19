# Extra-Configurations-Linux
*Usually the post configuration stuffs after my minimal installation*

## Printers (My case Brother DCP-T520w)

- Install cups
  ```
  $ sudo pacman -S cups
  ```
- Enable Cups
  ```
  $ sudo systemctl enable --now cups
  ```
- You have to be a member of lp
  - To list all the group just do
    ```
    $ groups
    ```
  - As root run this command. This adds our user to the group lp which allows to change the settings of our printer
    ```
    $ usermod -aG lp megame
    ```
- GUI for printer
  ```
  $ sudo pacman -S system-config-printer
  ```
  Then run the program
  
- Find the driver for your printer in my case
  ```
  $ git clone https://aur.archlinux.org/brother-dcpt520w.git
  $ cd brother-dcpt520w
  $ makepkg -si
  ```
- Then reopen the system-config-printer and scan for printers. Then the printer should appear

- References
  - https://youtu.be/jnmCbEWNV1w?si=jYH_kXoVGW_hiRhf
  - https://wiki.archlinux.org/title/CUPS/Printer-specific_problems
 

## Bluetooth

- Install the dependencies
```
$ sudo pacman -S bluez bluez-utils
```

- Check if bluetooth module is running or not
  ```
  $ lsmod | grep btusb
  ```
  Else load it using:
  ```
  $ sudo modprobe btusb
  ```
  
- Enable the bluetooth service
  ```
  $ sudo systemctl start bluetooth.service
  $ sudo systemctl enable bluetooth.service
  ```

- Then start the bluetooth using
  ```
  $ bluetoothctl
  ```
  You could use graphical interface like bluemann
  ```
  $ sudo pacman -S bluemann
  ```
- Then refer to this arch wiki page on how to connect to a bluetooth device etc: https://wiki.archlinux.org/title/bluetooth_headset#:~:text=Open%20GNOME%20Bluetooth%20and%20activate,when%20your%20device%20is%20connected.
  
- References
  - https://wiki.archlinux.org/title/bluetooth
  - https://youtu.be/rOL-T31l0lQ?si=FMz7CCFI5ykIohfu

## Opting in and out of a group

- To list the enrolled groups for a user
  ```
  $ groups <username>
  ```
- Opting in a group for a username
  ```
  $ sudo usermod -aG <service/program> <username>
  ```
- Opting out of a group for a username
  ```
  $ sudo gpasswd -d <username> <service/program>
  ```

## Loading drivers

- Loading a driver
  ```
  $ sudo modprobe <drivername>
  ```
- De-loading a driver
  ```
  $ sudo modprobe -R <drivername>
  ```

## Configuring wifi menu
- First git clone the repository
```
$ git clone https://github.com/ericmurphyxyz/rofi-wifi-menu.git
$ cd rofi-wifi-menu
```
- copy this file into local folder and make it executable
```
$ cp rofi-wifi-menu.sh ~/.local/bin/
$ cd ~/.local/bin/
$ chmod +x rofi-wifi-menu.sh
```
- Make a shortcut for it (~/.config/sxhkd/sxhkdrc -> BSPWM, ~/.xmonad/xmonad.hs -> XMonad)
```
alt+w
	rofi -show power-menu -modi power-menu:~/.local/bin/rofi-wifi-menu.sh
```
- References
  - https://github.com/ericmurphyxyz/rofi-wifi-menu

## Python packages

- Virutal environment
  - Installation
  ```
  $ sudo pacman -S python-virtualenv
  ```
  - For using cd into the python project you wanna execute then
  ```
  $ virtualenv -p /usr/bin/python3 yourenv
  $ source yourenv/bin/activate
  $ pip install package-name
  ```
- References
  - https://unix.stackexchange.com/questions/76389/recommended-way-of-installing-python-packages-on-arch

## Uninstalling packages completely
- Remove the packages completely using pacman
```
$ sudo pacman -Rn [package-name]
```
- If you wanna remove all config files related to it, here is where the config files would be stored for your package
```
~/.config
~/.local/share
~/.cache
/etc
/etc/xdg
/usr/share
```
## Basic controls (audio, video)

**Configuring audio**
- Installing pulseaudio
  ```
  $ sudo pacman -S pulseaudio alsa-utils pulseaudio-alsa
  $ pulseaudio --start
  ```
- Mapping keys to audio control (~/.config/sxhkd/sxhkdrc -> BSPWM, ~/.xmonad/xmonad.hs -> XMonad)
  ```
  #audio
  XF86AudioRaiseVolume
	  amixer set Master 2%+
  XF86AudioLowerVolume
	  amixer set Master 2%-
  XF86AudioMute
	  amixer set Master {mute, unmute}
  ```

**Configuring screenshot**
- Installing scrot
  ```
  $ sudo pacman -S scrot
  ```
- Mapping key to screenshot (~/.config/sxhkd/sxhkdrc -> BSPWM, ~/.xmonad/xmonad.hs -> XMonad)
  ```
  # Screenshot
  @Print
	  scrot -s --line mode=edge ~/Pictures/Screenshots/%Y-%m-%d_%H%M%S-$wx$h_screenshot-scrot.png
  ```
- Screenshot save destination
  ```
  $ mkdir ~/Pictures/
  $ mkdir ~/Pictures/Screenshots/
  ```

**Configuring screen brightnesss** (only works for brightness control displays like laptop displays)
- Installing brightnessctl
  ```
  $ sudo pacman -S brightnessctl
  ```
- Mapping keys to brightness control (~/.config/sxhkd/sxhkdrc -> BSPWM, ~/.xmonad/xmonad.hs -> XMonad)
  ```
  # Brightness
  #
  XF86MonBrightness{Up,Down}
	  brightnessctl s 350{+,-}
  ```
- References
  - https://youtu.be/EeDNuQO7TKE?si=hfqxLaNTpkrwtT_9
  - https://github.com/ilhamisbored/bspwm/blob/main/sxhkd/sxhkdrc

## To verify the system intergrity of files
- First we'll see if any systemctl services are failed
```
$ systemctl --failed
```
- First see where the system files are stores
```
$ lsblk
```
- First we need to unmount the files where the system files are stored
```
$ sudo umount /dev/sda2
```
- Scan for errors in the filesystem and automatically repair them
```
$ sudo fsck -p /dev/sda2
```
- Next remount the drive
```
$ sudo mount /dev/sda2
```

- References
  - https://linuxize.com/post/fsck-command-in-linux/
## Locating the file or package in linux
- Locating package
```
$ dpkg -L <package-name>
```
- Locating file
```
$ sudo apt-get install plocate
$ sudo updatedb
$ locate <filename>
```

## Installing packages using makefile
- To install first git clone the repo and cd into it. Then to install using makefile,
```
$ sudo make install
```
- To uninstall the packages using makefile first cd into the repo, then
```
$ sudo make uninstall
```
