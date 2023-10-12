---
title: About Monitoring
summary: General monitoring information
authors:
    - Jakob Zudrell
date: 2023-10-12
---
# About Monitoring
When my homelab started to grow and I was setting up more and more hardware (firewalls, switches, storage, virtualization, IoT, ...),
the need, or rather the interest to set up a centralized monitoring application rose.

For some reason I always stuck with Grafana and tools around it due to the great modularity where I could add single, small pieces
due to my requirements and not having one big service that does everything out of the box without me knowing what's really happening in the back.

However, as of today, the whole monitoring stuff has always been more of a "unstable" side project that sat there, did what I expected it to do, but
never got any fine tunings done. I set it up according to a few "getting started" docs and that's it.

Centralizing Logs and metric data is a highly complex process which needs a lot of planning. Of course you can
collect and store all the data (metrics, logs, ...) you get your hands on and basically that's what I do right now,
but then to efficiently read and interpret this data, you really need to put some thoughts into it. And probably that's
not even enough, due to the way of how for example Loki works with storing its logs together with labels and stuff, you ideally need to know about
how this works before setting everything up.

Long story short; This whole section about monitoring is at its very beginning and I need a lot more learning to improve it.
