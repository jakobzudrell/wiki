# Kodak i2000
Getting to work a Kodak i2000 on a Ubuntu VM was a classical approach of cherry picking the needed steps from the internet and hoping that it will work in the end.

These two posts did the trick for me (both in German)

* [https://forum.ubuntuusers.de/topic/alaris-s2050-usb-kodak-und-ubuntu-18-scanner-w/](https://forum.ubuntuusers.de/topic/alaris-s2050-usb-kodak-und-ubuntu-18-scanner-w/)
* [https://forum.ubuntuusers.de/topic/leben-einhauchen-fuer-kodak-i2400-duplexscanne/](https://forum.ubuntuusers.de/topic/leben-einhauchen-fuer-kodak-i2400-duplexscanne/)

## Environment
This project is done in a Ubuntu 20.04 server environment running on Proxmox. The Kodak scanner is connected via USB passthrough. 

## Installation
Here are the steps it took me to get everything to work.

1. Download the Kodak drivers from their [support site](https://support.alarisworld.com/en-us/i2400-scanner#Software)
```bash
wget https://resources.kodakalaris.com/docimaging/drivers/LinuxSoftware_i2000_v4.14.x86_64.deb.tar.gz
```
2. Download and install the multiarch-support package (this was needed for my 20.04 installation)
```bash
wget http://archive.ubuntu.com/ubuntu/pool/main/g/glibc/multiarch-support_2.27-3ubuntu1.5_amd64.deb
sudo dpkg -i multiarch-support_2.27-3ubuntu1.5_amd64.deb
````
```
I found out that the Kodak driver setup is quite sensitive when it comes to missing packages and other stuff. Therefore I just rebooted more or less after each step 😀
3. Next we install SANE
```bash
sudo apt install sane
```
4. And reboot again!
5. Extract and install the drivers - **unplug the scanner befor running the setup!**
```bash
tar -xvf LinuxSoftware_i2000_v4.14.x86_64.deb.tar.gz
sudo ./setup
```
6. Confirm the terms of conditions, do not install the Mono package and confirm the driver installation. You should see a success message in the end.
7. Create the symlinks as advised on the Kodak support page
```bash
sudo ln -sfr /usr/lib/sane/libsane-kds* /usr/lib/x86_64-linux-gnu/sane
```
8. Test your scanner capabilities using the `scanimage -A command.