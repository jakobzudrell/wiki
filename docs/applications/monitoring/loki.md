---
title: Loki
summary: Loki guides
authors:
    - Jakob Zudrell
date: 2023-10-12
---
# Loki
Loki is Grafana's approach to centralized logging. Send in logs from other clients for example using Promtail,
let Loki process and store them and then use Grafana for querying Loki to display nice graphs.

As with Grafana and Promtail, Loki gives endless options to configure it so I won't be posting any "recommended" configuration here.

## Installation
If you've added the Grafana APT repository to your machine you can easily install Loki using them.