# Network Automation with Ansible
## Description
This repository contains a set of Ansible playbooks designed to automate network configurations on Cisco routers and switches. It simplifies the setup of common network tasks such as IP addressing, VLAN configuration, and routing protocol setup. The goal is to reduce manual configuration time and improve consistency across network devices.

##Installation

###Prerequisites
Before using the palybooks, you must have the foloowing installed on your system:
- Ansible (v2.9+)
- Python 3.x
- Cisco devices or access to Cisco network simulator (e.g., GNS3, Cisco Packet Tracer)

- ###Steps to install
- 1. Clone the repository:
     ```bash
     git clone https://github.com/rollieson/Networking-Basics.git
     cd Networking-Basics

  2. Install dependencies:
     Install Ansible:
       sudo apt update
       sudo apt install ansible

     Install Python dependencies:
       pip install -r requirements.txt

  3. (Optional) Set up Ansible config file (ansible.cfg) for your environment.
  4. Ensure you have SSH access to the Cisco devices or a simulator
