---
title: Promtail
summary: Promtail guides
authors:
    - Jakob Zudrell
date: 2023-10-04
---
# Promtail
Quick guide on how I usually install and configure [Promtail](https://grafana.com/docs/loki/latest/send-data/promtail/configuration/#journal).

In a typical logging stack (as per Grafana's definition) Promtail is taking the role of a log collection agent that runs as a service on each monitored endpoint. By configuring it, you tell Promtail which sources to read logs from and where to send them. The target is usually a Loki server that is responsible for a centralized management of the sent in logs.

You can get an overview [here](https://grafana.com/docs/loki/latest/get-started/overview/).

## Installation
The [documentation over at Grafana Labs](https://grafana.com/docs/loki/latest/send-data/promtail/installation/) is not really detailed when it comes to installing, so here's my way.

1. Head over to the Promtail [releases page](https://github.com/grafana/loki/releases) and download the latest build for your platform and unzip it.
```bash
wget https://github.com/grafana/loki/releases/download/v2.9.1/promtail-linux-amd64.zip
unzip promtail-linux-amd64.zip
```
2. I like to put my downloaded binaries to `/opt/`, were also renaming the binary to just `promtail` in this step, which I think makes it look nicer.
```bash
sudo mkdir /opt/promtail
sudo mv promtail-linux-amd64 /opt/promtail/promtail
```
3. Create a symlink to the binary, just so we have it on PATH which makes it a little nicer to work with it
```bash
sudo ln -s /opt/promtail/promtail /usr/local/bin/
```
4. Lastly for the installation we will create a system user for Promtail under which the app will run as a service later. Also we need to make sure the newly created user has read access to the journal (as per [the manpage](https://manpages.debian.org/buster/systemd/systemd-journald.service.8.en.html#ACCESS_CONTROL)).
```bash
sudo useradd --system promtail
sudo usermod -aG systemd-journal promtail
```

Basically that's it. Promtail is now installed on our system and theoretically ready to run. However there is some more configuration needed to make sure Promtail behaves the way you want it to. In the next steps we're gonna create a configuration for the application and a **systemd** service to make sure Promtail always runs in the back.

## Configuration
Promtail is configured using a YAML configuration file, which I always put at `/etc/promtail/config.yaml`. An example is at the bottom of this page.
Configuration refeference can be found [here](https://grafana.com/docs/loki/latest/send-data/promtail/configuration/).

To setup a systemd service I put the following contents into `/etc/systemd/system/promtail.service`:
```
[Unit]
Description=Promtail Service
After=network.target

[Service]
Type=simple
User=promtail

# Add an environment variable with the hostname to use in labels
Environment="HOSTNAME=%H"
ExecStart=/usr/local/bin/promtail -config.file /etc/promtail/config.yaml -config.expand-env=true

# Give a reasonable amount of time for promtail to start up/shut down
TimeoutSec = 60
Restart = on-failure
RestartSec = 2

[Install]
WantedBy=multi-user.target
```

Reload the systemd-daemon, start the service and see if it works;

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now promtail.service
sudo systemctl status promtail
```

### Example
An example `config.yaml` may look like this. Please note that I am also just starting out with Grafana and Promtail so do not take this as a best practice but rather as a "it works" reference.
```yaml
server:
  disable: true

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://yourslokiserver:3100/loki/api/v1/push

scrape_configs:
  - job_name: journal
    journal:
      json: true
      max_age: 12h
      labels:
        job: systemd-journal
        instance: ${HOSTNAME}
    relabel_configs:
      - source_labels: ['__journal__systemd_unit']
        target_label: 'unit'
```

## Ansible
I will be trying to automate this process as sonn as I am confident it works well enough. Stay tuned ;)