# setup-floodlight-sdn
A step-by-step guide to installing and configuring the Floodlight SDN controller. Includes dependencies, setup instructions, and troubleshooting tips.
Sure! Here's your text in Markdown with the English translation:
# Abstract
This document examines and provides a guide on how to install and use the Floodlight controller (an SDN-based controller) and the Mininet network simulator. The main goal is to provide a comprehensive guide for beginners and enthusiasts in the field of Software-Defined Networks (SDN).

# Introduction
Software-Defined Networks (SDN) enable dynamic and flexible network management by separating the control layer from the data layer. This document teaches how to install and implement an SDN-based network using Floodlight (as the controller) and Mininet (as the network simulator).

# Prerequisites
- **Operating System**: Linux-based distributions (such as Ubuntu, Fedora, or Arch).
- **Python**: Version 3 or higher.
- **Development Tools**: git, pip, openvswitch, build-essential.

## Installing Prerequisites on Ubuntu/Debian:
```bash
sudo apt update
sudo apt install git python3 python3-pip openvswitch-switch build-essential
```

# Installing Mininet
## Installation Steps for Arch-Based Distributions:
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

# Check successful installation
sudo mn --test pingall
```

# Installing Floodlight
### Prerequisites:
- Java version 8 or higher.
- Build tools (maven).

### Installation Steps:
```bash
# Clone the Floodlight repository
git clone https://github.com/floodlight/floodlight.git
cd floodlight/
git submodule init
git submodule update

# Build the project using Maven
mvn clean install

# Run Floodlight
java -jar target/floodlight.jar
```

# Using Floodlight
## Connecting Mininet to Floodlight
1. Run Floodlight (default port: 6653).
2. Create a simple topology in Mininet:

```bash
sudo mn --controller=remote,ip=127.0.0.1,port=6653 --switch ovsk,protocols=OpenFlow13 --topo=single,3
```

## Firewall Management with API
### Enable Firewall:
```bash
curl -X POST http://localhost:8080/wm/firewall/module/enable/json
```

### Add ICMP rule between two hosts:
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

# Conclusion
This project demonstrated how to simulate and manage SDN networks using Floodlight and Mininet. The main advantages of this approach include:

- Centralized network management.
- Reduced hardware costs.
- Flexibility in applying security policies.

For more details, refer to the official Floodlight documentation.
```

Let me know if you'd like any adjustments!
