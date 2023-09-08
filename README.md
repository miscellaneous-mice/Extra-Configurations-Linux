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
