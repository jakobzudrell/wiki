---
title: Debian
summary: About the Debian Operating System
authors:
    - Jakob Zudrell
date: 2023-10-03
---
# Debian
## Autoinstall
Autoinstall for Debian systems is done by preseeding the [DebianInstaller](https://wiki.debian.org/DebianInstaller/Preseed).

An iPXE (documentation TBD) server boots a Debian netboot image providing a preseed configuration as seen below.
```
set base http://pxe.zudrell.eu/isos/debian-12.1.0-amd64-netboot
kernel ${base}/debian-installer/amd64/linux 
initrd ${base}/debian-installer/amd64/initrd.gz
initrd ${base}/preseed.cfg preseed.cfg
boot
```

An example preseed configuration can be obtained from the Debian wiki. Right now in my case the only thing needed to enter manually during installation is the hostname of the machine. In such a small environment I did not bother automating this step.

Here are some of the notable steps performed in the preseed file. At some point I might upload the whole file to the Github repo but for now I want to improve it a little further.

 * **User account creation** - Creation of root account is skipped. A default user with sudo privileges and a predefined UID is created. This is necessary because I do not have any LDAP solution in place.
 * **Partitioning** - As I am only using this on my Proxmox host right now always a full disk will be used.
 * **Packages** - The following packages are installed using the `d-i pkgsel/include string` option
 ```
 build-essential git htop bash-completion net-tools bsd-mailx apticron unattended-upgrades tcpdump
 ```

In the end the installer sets up an `authorized_keys` file to allow pubkey authentication via SSH, installs my certificate chain and executes a comprehensive **firstrun** bash script for further setup which will probably be replaced by Ansible in the future to keep things more flexible and *idempotent* ;)
```
d-i preseed/late_command string in-target mkdir -p /home/youruser/.ssh; \
in-target wget http://pxe.zudrell.eu/isos/debian-12.1.0-amd64-netboot/config/authorized_keys -O /home/youruser/.ssh/authorized_keys; \
in-target chown -R youruser:youruser /home/youruser/.ssh/; \
in-target chmod 600 /home/youruser/.ssh/authorized_keys; \
in-target chmod 700 /home/youruser/.ssh/; \
in-target wget http://certdata.zudrell.eu/ZUDRELLrootCA.crt -P /usr/local/share/ca-certificates; \
in-target wget http://certdata.zudrell.eu/ZUDRELLidentityCA.crt -P /usr/local/share/ca-certificates; \
in-target update-ca-certificates; \
in-target wget http://pxe.zudrell.eu/isos/debian-12.1.0-amd64-netboot/firstrun.sh -O /tmp/firstrun.sh; \
in-target chmod 754 /tmp/firstrun.sh; \
in-target /bin/bash /tmp/firstrun.sh
```