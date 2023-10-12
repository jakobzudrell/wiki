---
title: Prometheus
summary: Prometheus guides
authors:
    - Jakob Zudrell
date: 2023-10-09
---
# Prometheus
TBD

## Exporters
### Blackbox Exporter
```
# Download and unpack the latest release
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.24.0/blackbox_exporter-0.24.0.linux-amd64.tar.gz
tar -xvf blackbox_exporter-0.24.0.linux-amd64.tar.gz

# Move it to the /opt directory
sudo mv blackbox_exporter-0.24.0.linux-amd64 /opt/blackbox_exporter

# Create symlink
sudo ln -s /opt/blackbox_exporter/blackbox_exporter /usr/local/bin/blackbox_exporter

# Move the configuration to /etc/prometheus
# Note: you could create a separate directory for the app but I'll go with prometheus
sudo mv blackbox.yml /etc/prometheus/
```

Systemd unit file:
```
[Unit]
Description=Blackbox Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=blackbox_exporter --config.file='/etc/prometheus/blackbox.yml'

[Install]
WantedBy=multi-user.target
```