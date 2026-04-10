# Build a Home Lab Like a Pro

[![Blog Post](https://img.shields.io/badge/Blog-NetworkThinkTank-blue)](https://networkthinktank.blog)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Lab Series](https://img.shields.io/badge/Lab%20Series-NetworkThinkTank--Labs-green)](https://github.com/NetworkThinkTank-Labs)

> A comprehensive guide to building a professional home networking lab - from hardware selection to virtualization platforms to hands-on lab exercises.
>
> **Companion Blog Post:** [Build a Home Lab Like a Pro](https://networkthinktank.blog/build-a-home-lab-like-a-pro/) on NetworkThinkTank.blog
>
> ---
>
> ## Table of Contents
>
> - [Overview](#overview)
> - - [Repository Structure](#repository-structure)
>   - - [Hardware Recommendations](#hardware-recommendations)
>     - - [Virtualization Platforms](#virtualization-platforms)
>       - - [Sample Topologies](#sample-topologies)
>         - - [Software Tools](#software-tools)
>           - - [Getting Started](#getting-started)
>             - - [Related Labs](#related-labs)
>               - - [Contributing](#contributing)
>                 - - [License](#license)
>                  
>                   - ---
>
> ## Overview
>
> This repository is the companion resource for the **"Build a Home Lab Like a Pro"** blog post on [NetworkThinkTank.blog](https://networkthinktank.blog). It contains:
>
> - **Hardware buying guides** with tiered budget recommendations
> - - **Sample configuration files** for common lab setups
>   - - **Network topology diagrams** (draw.io source files included)
>     - - **Scripts** for lab automation and environment setup
>       - - **Topology definition files** for GNS3, EVE-NG, and Containerlab
>        
>         - Whether you are studying for your CCNA, building a data center simulation, or testing automation scripts, this repo gives you a head start.
>        
>         - ---
>
> ## Repository Structure
>
> ```
> home-lab-guide/
> |-- README.md
> |-- LICENSE
> |-- diagrams/
> |   |-- starter-lab-topology.drawio
> |   |-- enterprise-lab-topology.drawio
> |   |-- datacenter-fabric-topology.drawio
> |   |-- home-lab-physical-layout.drawio
> |-- configs/
> |   |-- firewall/
> |   |   |--- pfsense-base-config.xml
> |   |   |-- opnsense-vlan-setup.md
> |   |-- switching/
> |   |   |-- vlan-trunk-config.txt
> |   |   |-- spanning-tree-best-practices.txt
> |   |-- routing/
> |   |   |-- ospf-area-config.txt
> |   |   |-- bgp-ebgp-peering.txt
> |   |   |-- hsrp-vrrp-config.txt
> |   |-- automation/
> |       |-- ansible-inventory-example.yml
> |       |-- netmiko-backup-script.py
> |-- topologies/
> |   |-- containerlab/
> |   |   |-- starter-lab.clab.yml
> |   |   |-- enterprise-lab.clab.yml
> |   |   |-- datacenter-fabric.clab.yml
> |   |-- gns3/
> |   |   |-- starter-lab.gns3project
> |   |-- eve-ng/
> |       |-- topology-import-guide.md
> |-- scripts/
> |   |-- lab-setup/
> |   |   |-- install-proxmox-post.sh
> |   |   |-- install-containerlab.sh
> |   |   |-- install-gns3-server.sh
> |   |   |-- install-docker-tools.sh
> |   |-- monitoring/
> |   |   |-- deploy-librenms.sh
> |   |   |-- deploy-grafana-stack.sh
> |   |-- utilities/
> |       |-- generate-lab-configs.py
> |       |-- cleanup-lab.sh
> |-- docs/
>     |-- hardware-buying-guide.md
>     |-- platform-comparison.md
>     |-- budget-breakdown.md
>     |-- lab-exercise-workflow.md
> ```
>
> ---
>
> ## Hardware Recommendations
>
> ### Tier 1: Budget Build ($200-$500)
> | Component | Recommendation | Est. Cost |
> |-----------|---------------|-----------|
> | Server | Dell OptiPlex 7050/7060 or HP EliteDesk 800 G3 | $100-$200 |
> | RAM | 32 GB DDR4 | $40-$60 |
> | Storage | 500 GB SSD + 1 TB HDD | $50-$80 |
> | Switch | Netgear GS308T or TP-Link TL-SG108E | $30-$50 |
>
> ### Tier 2: Intermediate Build ($500-$1,500)
> | Component | Recommendation | Est. Cost |
> |-----------|---------------|-----------|
> | Server | Dell PowerEdge R720/R730 or HP DL380 Gen9 | $200-$500 |
> | RAM | 64-128 GB DDR4 ECC | $100-$200 |
> | Storage | 1 TB NVMe + 2 TB HDD | $100-$200 |
> | NIC | Intel X520-DA2 10GbE | $30-$50 |
> | Firewall | Netgate SG-1100 or OPNsense box | $100-$200 |
>
> ### Tier 3: Advanced Build ($1,500+)
> | Component | Recommendation | Est. Cost |
> |-----------|---------------|-----------|
> | Servers (x2-3) | Dell R740xd or HP DL380 Gen10 | $500-$1K each |
> | RAM | 128-256 GB per node | $200-$400 each |
> | NAS | TrueNAS or Synology for shared storage | $300-$600 |
>
> See [docs/hardware-buying-guide.md](docs/hardware-buying-guide.md) for detailed recommendations.
>
> ---
>
> ## Virtualization Platforms
>
> | Platform | Cost | Best For | Resource Usage |
> |----------|------|----------|----------------|
> | **Proxmox VE** | Free | All-in-one virtualization host | Medium |
> | **VMware ESXi** | Free* | Enterprise environments | Medium |
> | **GNS3** | Free | Cisco-centric labs, cert prep | High |
> | **EVE-NG** | Free/Paid | Multi-vendor complex topologies | High |
> | **Containerlab** | Free | Automation, CI/CD, quick labs | Low |
>
> See [docs/platform-comparison.md](docs/platform-comparison.md) for a detailed comparison.
>
> ---
>
> ## Sample Topologies
>
> ### Starter Lab (Containerlab)
>
> ```yaml
> name: starter-lab
> topology:
>   nodes:
>     firewall:
>       kind: linux
>       image: ghcr.io/srl-labs/alpine-ftp-server
>     core-switch:
>       kind: linux
>       image: ghcr.io/srl-labs/network-multitool
>     server1:
>       kind: linux
>       image: alpine:latest
>     server2:
>       kind: linux
>       image: alpine:latest
>   links:
>     - endpoints: ["firewall:eth1", "core-switch:eth1"]
>     - endpoints: ["core-switch:eth2", "server1:eth1"]
>     - endpoints: ["core-switch:eth3", "server2:eth1"]
> ```
>
> ### Data Center Fabric (Containerlab)
>
> ```yaml
> name: dc-fabric
> topology:
>   nodes:
>     spine-01:
>       kind: linux
>       image: frrouting/frr:latest
>     spine-02:
>       kind: linux
>       image: frrouting/frr:latest
>     leaf-01:
>       kind: linux
>       image: frrouting/frr:latest
>     leaf-02:
>       kind: linux
>       image: frrouting/frr:latest
>   links:
>     - endpoints: ["spine-01:eth1", "leaf-01:eth1"]
>     - endpoints: ["spine-01:eth2", "leaf-02:eth1"]
>     - endpoints: ["spine-02:eth1", "leaf-01:eth2"]
>     - endpoints: ["spine-02:eth2", "leaf-02:eth2"]
> ```
>
> ---
>
> ## Software Tools
>
> ### Monitoring and Management
> - [LibreNMS](https://www.librenms.org/) - Network monitoring via SNMP
> - - [Grafana](https://grafana.com/) + [Prometheus](https://prometheus.io/) - Metrics and dashboards
>   - - [Wireshark](https://www.wireshark.org/) - Packet analysis
>    
>     - ### Automation
>     - - [Ansible](https://www.ansible.com/) - Configuration management
>       - - [Python](https://www.python.org/) + [Netmiko](https://github.com/ktbyers/netmiko) - Device scripting
>         - - [Git](https://git-scm.com/) - Version control for configs
>          
>           - ### Infrastructure
>           - - [Pi-hole](https://pi-hole.net/) - DNS + ad blocking
>             - - [NetBox](https://netbox.dev/) - IPAM and DCIM
>               - - [Docker](https://www.docker.com/) - Container platform
>                
>                 - ---
>
> ## Getting Started
>
> ### Quick Start with Containerlab
>
> ```bash
> # 1. Install Containerlab
> bash -c "$(curl -sL https://get.containerlab.dev)"
>
> # 2. Clone this repository
> git clone https://github.com/NetworkThinkTank-Labs/home-lab-guide.git
> cd home-lab-guide
>
> # 3. Deploy the starter topology
> cd topologies/containerlab
> sudo containerlab deploy -t starter-lab.clab.yml
>
> # 4. Access your nodes
> docker exec -it clab-starter-lab-core-switch bash
>
> # 5. When done, tear down
> sudo containerlab destroy -t starter-lab.clab.yml
> ```
>
> ### Quick Start with Proxmox
>
> 1. Download [Proxmox VE ISO](https://www.proxmox.com/en/downloads)
> 2. 2. Install on your lab server
>    3. 3. Run the post-install script: `bash scripts/lab-setup/install-proxmox-post.sh`
>       4. 4. Create VMs for your network devices
>          5. 5. Follow the topology guides in `docs/`
>            
>             6. ---
>            
>             7. ## Related Labs
>            
>             8. This guide integrates with the full **NetworkThinkTank-Labs** series:
>
> | Lab | Description | Link |
> |-----|-------------|------|
> | Lab 01 | BGP Fundamentals | [lab-01-bgp-fundamentals](https://github.com/NetworkThinkTank-Labs/lab-01-bgp-fundamentals) |
> | Lab 02 | VPN with IPSec and GRE | [lab-02-vpn-ipsec-gre](https://github.com/NetworkThinkTank-Labs/lab-02-vpn-ipsec-gre) |
> | Lab 03 | EVPN-VXLAN Fabric | [lab-03-evpn-vxlan](https://github.com/NetworkThinkTank-Labs/lab-03-e3-evpn-vxlan) |
> | Lab 04 | Network Automation (Ansible) | [lab-04-network-automation-ansible](https://github.com/NetworkThinkTank-Labs/lab-04-network-automation-ansible) |
> | Bonus | Network Backup Automation | [network-backup-automation](https://github.com/NetworkThinkTank-Labs/network-backup-automation) |
>
> ---
>
> ## Contributing
>
> Contributions are welcome! If you have:
> - Additional topology examples
> - - Hardware recommendations
>   - - Platform-specific tips
>     - - Bug fixes or improvements
>      
>       - Please open an issue or submit a pull request.
>      
>       - ---
>
> ## License
>
> This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
>
> ---
>
> **Built by [NetworkThinkTank](https://networkthinktank.blog) | Part of the [NetworkThinkTank-Labs](https://github.com/NetworkThinkTank-Labs) series**
