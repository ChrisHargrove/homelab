---
tags:
  - app
  - monitoring
---
This is a solo deployment that helps provision [[Prometheus]] and some of its components.
[Link](https://github.com/prometheus-operator/prometheus-operator)

This provides a Kubernetes native deployment for the deployment and management of Prometheus and related monitoring components.

Features:
- [[Custom Resource Definition|Kubernetes Custom Resources]] - This allows prometheus to extend the kubernetes Api
- Simplified Deployment Configuration - Helps with the configuration of the Prometheus fundamentals sich as versions, persistence, retention policies and replica management.