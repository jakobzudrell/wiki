---
title: Grafana
summary: Grafana guides
authors:
    - Jakob Zudrell
date: 2023-10-09
---
# Grafana
Grafana is a monitoring frontend that lets you query and display a lot of different data sources (e.g. Prometheus, Loki, ...).
In my setup Grafana is currently used for monitoring my servers via different Prometheus Exporters and for centralized loggin using Loki & Promtail.

## Installation
The installation is quite straightforward and can be done via APT. Please refer to the [official docs](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/) for the installation.

## Usage
Using Grafana is fairly simple. Installation via APT package takes care of all the basic setup and in the end you can control Grafana using the `systemctl` and the `grafana-server` unit.

Make sure Grafana is running and starts automatically on boot:
```
sudo systemctl enable --now grafana-server.service
```

## Configuration
The main configuration file is `/ect/grafana/grafana.ini`. It features an incredible amount of options so you are better of with the offical configuration reference.