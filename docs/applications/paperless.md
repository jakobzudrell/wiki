---
title: Paperless
summary: paperless-ngx installation & other guides
authors:
    - Jakob Zudrell
date: 2023-10-02
---
# paperless-ngx

## Installation
TBD

The application is installed on a LXC container and uses a remote MySQL instance. Files are stored via an NFS mounted share.

## Updating
As per 2023-07-12 a paperless-ngx update can be applied by following these steps, assuming the application directory is located at `/opt/paperless` and you are running the app as user the `paperless`.


```bash
# Download the latest release -> make sure to download the latest release
wget https://github.com/paperless-ngx/paperless-ngx/releases/download/v1.14.4/paperless-ngx-v1.14.4.tar.xz

# Backup the existing installation
cp -r /opt/paperless /opt/paperless.bak

# Extract downloaded release and copy it to the installation dir
tar -xf paperless-ngx-v1.14.4.tar.xz
cp -r paperless-ngx/* /opt/paperless/

# Change to paperless dir and set ownerships
cd /opt/paperless
chown -R paperless:paperless .

# Install requirements
sudo -Hu paperless pip install -r requirements.txt

# Remove default config
rm ./paperless.conf

# Migrate db schema
sudo -Hu paperless python3 manage.py migrate

# Restart services
sudo systemctl restart paperless-consumer.service paperless-task-queue.service paperless-scheduler.service paperless-webserver.service
```