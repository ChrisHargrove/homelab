---
tags:
  - k3s
---
A custom operator is a [[Custom Controller|controller]] but it usually has a more nuanced role. They normally have operational logic for managing a specific application or service in [[Kubernetes]] 
Operators follow a pattern of managing complex applications where they automate lifecycle tasks such as:
- Installation
- Configuration
- Scaling
- Backup/Restore
- Upgrades
- Failover/Recovery

The key distinction between a controller and operator is that an operator goes beyond just managing the state of Kubernetes objects - it encapsulates the domain-specific logic needed to manage the entire application.