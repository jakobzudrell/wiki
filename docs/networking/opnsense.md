---
title: OPNsense
summary: Stuff related to OPNsense
authors:
    - Jakob Zudrell
date: 2023-10-02
---
# ![OPNsense](../assets/logos/opnsense.png)

This page is all about [OPNsense](https://opnsense.org/)

## GeoIP
For some use cases it might be helpful to access a GeoIP service. For example if you want to block specific countries on your firewall WAN interface to not even let them go anywhere.

!!! info
    Some services might stop working when applying such rules. An example would be Let'sEncrypt certificate renewal via HTTP challenge.

GeoIP will be set up as an alias, which queries the Maxmind service for current date.
Since the topic is already well explained you can just head [over there](https://docs.opnsense.org/manual/how-tos/maxmind_geo_ip.html) and take a look yourself.