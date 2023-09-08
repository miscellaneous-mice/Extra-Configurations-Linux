# Extra-Configurations-Linux
*Usually the post configuration stuffs after my minimal installation*

## Printers (My case Brother DCP-T520w)

- Install cups
  ```
  sudo pacman -S cups
  ```
- Enable Cups
  ```
  sudo systemctl enable --now cups
  ```
- You have to be a member of lp
  - To list all the group just do
    ```
    groups
    ```
  - As root run this command. This adds our user to the group lp which allows to change the settings of our printer
    ```
    usermod -aG lp megame
    ```
- GUI for printer
  ```
  sudo pacman -S system-config-printer
  ```
  Then run the program
  
- Find the driver for your printer in my case
  ```
  git clone https://aur.archlinux.org/brother-dcpt520w.git
  cd brother-dcpt520w
  makepkg -si
  ```
- Then reopen the system-config-printer and scan for printers. Then the printer should appear

## Bluetooth

- Install the dependencies
```
sudo pacman -S bluez bluez-utils
```

- Check if bluetooth module is running or not
  ```
  lsmod | grep btusb
  ```
  Else load it using:
  ```
  sudo modprobe btusb
  ```
- You should add your user to group
  ```
  sudo groupadd bluetooth
  sudo usermod -a -G bluetooth <loginuser>
  ```
- Enable the bluetooth service
  ```
  sudo systemctl start bluetooth.service
  sudo systemctl enable bluetooth.service
  ```

- Then start the bluetooth using
  ```
  bluetoothctl
  ```
  You could use graphical interface like bluemann
  ```
  sudo pacman -S bluemann
  ```
- Then refer to this arch wiki page on how to connect to a bluetooth device etc
  ```
  https://wiki.archlinux.org/title/bluetooth_headset#:~:text=Open%20GNOME%20Bluetooth%20and%20activate,when%20your%20device%20is%20connected.
  ```

## Opting in and out of a group

- To list the enrolled groups for a user
  ```
  groups <username>
  ```
- Opting in a group for a username
  ```
  sudo usermod -aG <service/program> <username>
  ```
- Opting out of a group for a username
  ```
  sudo gpasswd -d t<username> <service/program>
  ```

## Python packages

- Virutal environment
  - Installation
  ```
  sudo pacman -S python-virtualenv
  ```
  - For using cd into the python project you wanna execute then
  ```
  virtualenv -p /usr/bin/python3 yourenv
  source yourenv/bin/activate
  pip install package-name
  ```
