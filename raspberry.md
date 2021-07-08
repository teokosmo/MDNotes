# Raspberry Pi <!-- omit in toc -->

- [configure network interface](#configure-network-interface)
- [setup  file server](#setup-file-server)

## configure network interface

[Reference](https://www.raspberrypi.org/documentation/configuration/tcpip/)

- Run `ifconfig` to check network configuration
- Run `sudo ifconfig wlan0 down` to disable wlan0 interface (wifi), more on [raspberrytips](https://raspberrytips.com/disable-wifi-raspberry-pi/)

## setup  file server

[Reference](https://raspberrytips.com/raspberry-pi-file-server/)

- Add static ip (i.e. 192.168.1.5) for raspberry in router's DHCP server, [router UI](http://192.168.1.1)
- Enable ssh in raspberry
  `sudo service ssh start`
- Connect to raspberry from any other machine
  `ssh pi@192.16.1.5`
- Install Samba (in raspberry)
  
  ```bash
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get install samba
  ```

- Configure Samba
  - give all permissions to folder that will be shared `sudo chmod 777 /media/pi/INTENSO`
  - edit /etc/samba/smb.conf by alling following lines at the end
    ```txt
    [PI] 
    comment = RaspberryPi
    public = no
    writeable = yes
    browsable = yes 
    path = /media/pi/INTENSO
    create mask = 0777
    directory mask = 0777
    ```
  - enable raspberry user `pi` to connect to samba shared folder `sudo smbpasswd -a pi`
- Restart Samba `sudo service smbd restart`
- Access shared folder
  - from Windows \\192.168.1.5\PI
  - from Linux/Mac smb://192.168.1.5/PI
