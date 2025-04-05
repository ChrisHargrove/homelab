This node is the most recent release of Raspberry Pi. [Link](https://thepihut.com/products/raspberry-pi-5?srsltid=AfmBOoolq_WCDWZSfYQiNYAca3yogbm7xLJNOWkTpp4fNddDTLSTHz7_&variant=42531604955331)

## Features
- 64bit Quad-Core Arm Cortex-A76 CPU
- 8Gb RAM
- 64Gb SD Card
- 1Gb Networking
- PCI-E Compatible 3rd Gen

### Notes
Currently this Pi operates using the SD Card for the bootable OS. This is not a long term solution as the SD Card will degrade over time and when using Kubernetes it relies on etcd which can degrade the SD Card during operation.

Going forward the SD Card needs replacing with a Pi Hat that provides a bootable NVM-E drive that can have a larger capacity as well as bandwidth thanks to the PCI-E features.[Single NVMe](https://shop.pimoroni.com/products/nvme-base?variant=41219587178579) [Dual NVMe](https://shop.pimoroni.com/products/nvme-base-duo-for-raspberry-pi-5?variant=41434434895955) 