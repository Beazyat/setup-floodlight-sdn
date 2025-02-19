# setup-floodlight-sdn
A step-by-step guide to installing and configuring the Floodlight SDN controller. Includes dependencies, setup instructions, and troubleshooting tips.

# Floodlight Controller and Mininet Installation Guide

## Abstract
This document provides a detailed guide on installing and using the Floodlight controller (an open-source SDN controller) and the Mininet network simulator. The primary aim is to offer a comprehensive guide for beginners and those interested in Software-Defined Networking (SDN).

## Introduction
Software-Defined Networking (SDN) enables dynamic and flexible network management by separating the control plane from the data plane. This document demonstrates how to install and implement an SDN network using Floodlight (as the controller) and Mininet (as the network simulator).

## Prerequisites
- **Operating System**: Linux-based distributions (e.g., Ubuntu, Fedora, or Arch).
 This tutorial is for the Arch base distro and install on another distro a little different.
- **Python**: Version 3 or higher.
- **Development Tools**: Git, pip, OpenvSwitch, build-essential.

## Installing Prerequisites on Ubuntu/Debian
```bash
sudo apt update
sudo apt install git python3 python3-pip openvswitch-switch build-essential
```

## Installing Mininet
### Steps for Arch-based distributions:
```bash
# Install prerequisites
sudo pacman -S net-tools iperf ethtool openvswitch

# Install libcgroup from AUR
git clone https://aur.archlinux.org/libcgroup.git
cd libcgroup-git/
makepkg -sri

# Install Mininet from AUR
git clone https://aur.archlinux.org/mininet.git
cd mininet/
makepkg -sri

# Verify the installation
sudo mn --test pingall
```

## Installing Floodlight
### Prerequisites:
- Java version 8 or higher.
- Build tools (Maven).

### Installation steps:
```bash
# Clone the Floodlight repository
git clone https://github.com/floodlight/floodlight.git
cd floodlight/
git submodule init
git submodule update

# Build the project with Maven
mvn clean install

# Run Floodlight
java -jar target/floodlight.jar
```

### For check app is worked search this link:
http://localhost:8080/ui/pages/index.html

## Using Floodlight
### Connecting Mininet to Floodlight
1. Run Floodlight (default port: 6653).
2. Create a simple topology in Mininet:

```bash
sudo mn --controller=remote,ip=127.0.0.1,port=6653 --switch ovsk,protocols=OpenFlow13 --topo=single,3
```

## Managing the Firewall via API
### Enabling the Firewall:
```bash
curl -X POST http://localhost:8080/wm/firewall/module/enable/json
```

### Adding an ICMP rule between two hosts:
```bash
# Allow traffic from h1 to h2
curl -X POST http://localhost:8080/wm/firewall/rules/json \
-H "Content-Type: application/json" \
-d '{
    "src-ip": "10.0.0.1/32",
    "dst-ip": "10.0.0.2/32",
    "nw-proto": "ICMP",
    "action": "ALLOW"
}'
```

## Conclusion
This project demonstrated how to simulate and manage SDN networks using Floodlight and Mininet. The main benefits of this approach include:

- Centralized network management.
- Reduced hardware costs.
- Flexibility in applying security policies.

For more details, refer to the official Floodlight documentation.
https://floodlight.atlassian.net/wiki/spaces/floodlightcontroller/overview
https://github.com/floodlight/floodlight

