# Network Namespace with Open vSwitch

This project demonstrates how to set up a virtual switch using Open vSwitch (OVS) and configure network namespaces in Linux. It includes steps to create a virtual switch, set up two network namespaces, connect them to the switch, and assign IP addresses.

## Prerequisites

Ensure you have the following installed on your Linux system:

- Open vSwitch (OVS)
- `iproute2` package

You can install these packages using your package manager. For example, on Ubuntu, you can use:

```bash
sudo apt update
sudo apt install openvswitch-switch iproute2
