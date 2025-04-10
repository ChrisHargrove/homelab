---
tags:
  - app
  - monitoring
---
Prometheus is a collector of metrics, it uses something called service monitors that provide data that Prometheus can scrape. All of this information will use persistent storage that we can configure for how long we want it kept.

The parts that are required to get Prometheus setup are:
- The [[Prometheus Operator]]
- The Service Monitors
- Prometheus itself
- Grafana

For this setup I am not going to use the helm chart for installation as it installed a whole bunch of things that have to be configured using a values.yaml file. And in the end I wont truly have a good understanding of how the monitoring stack works.