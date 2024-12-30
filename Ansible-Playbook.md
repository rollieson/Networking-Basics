# WHat is an Ansible Playbook?
An Ansible Playbook is a YAML fil that defines a series of tasks that need to be executed on one or more managed hosts. Each task uses an Ansible module to perform an action on the host. A playbook can be used to automate a wide variety of tasks like system configurtaion, application deployment, and network automation.
## A playbook consists of the following key components:
1. Hosts: The target systems or group of systems where the playbook will run.
2. Tasks: The actions that need to be performed on the target systems.
3. Modules: The tools that Ansible uses to carry out the tasks.
4. Variables: Values that can be reused in tasks, making the playbook more flexible and dynamic.
5. Handlers: Special tasks that are triggered only when notified by another task.

# Basic Structure of a Playbook
A playbook starts with a listof plays. Each play maps a group of host to a set of tasks to be executed on those hosts. The basic structure looks like this:

```
- name: Install and configure Nginx
  hosts: webservers
  become: yes
  tasks:
  - name: Install nginx package
    ansible.builtin.yum:
    name: nginx
    state: present

  - name: Start nginx service
    ansible.builtin.service:
    name: nginx
    state: started
    enabled: yes
```

## Explanation
- name: A descriptive name for the play.
- hosts: The target group orhost(s) on which the tasks will run (can be a group defined in your inventory file or localhost).
- become: Elevates privilege (similar to sudo).
- tasks: A list of tasks, each with a name and module (e.g., ansible.builtin.yum for package management).

## Key components:
- Tasks: These are theactions to be performed, suchas installing a package, copying files, or configuring services.
- Modules: Ansible modules are the building blocks for performing tasks. Common modules include yum, apt, service, file, and command.


# Defining Hosts: Inventory File
The inventory file defines the systems (host) on which Ansible will perform actions. It can be a simple static file or dynamically generated based on yout environment. Here is a sample static inventory file:
```
[webservers]
web1.example.com
web2.exampole.com

[dbservers]
db1.example.com
db2.example.com
```
This inventory defines two groups:
- webservers: A group of web servers that the playbook will target.
- dbservers: A group of database servers.

You can then target these groups in your playbook by using hosts: webservers or hosts: dbservers.

# Ansible Modules
Modules are the core of ansible. Each module is designed to handle specific tasks like managing packages, services, files, and more. Some common modules you might use in playbooks include:
- ansible.builtin.yum: Used to manage packages on red Hat-based systems (CentOs, Fedora, RHEL).
- ansible.builtin.apt: Used to manage packages on Debian-based systems (Ubuntu, Debian).
- ansible.builtin.service: Used to manage services (start, stop, restart, enable, disable).
- ansible.builtin.copy: Used to copy files to the target hosts.
- ansible.builtin.command: Runs arbitrary commands on the target hosts.

Example using apt module to install a package:
```
- name: Install nginx on Ubuntu server
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      ansible.builtin.apt:
      name: nginx
      state: present
```

# Using Variables in Playbooks
Variable allow you to make your playbooks more flexible. you can define variables in several places:
- Directly in the playbook
- In an external file (e.g., .yml files).
- In the command line when running the playbook.

## Defining Variables
```
- name: Install and configure Nginx
  hosts: webservers
  become: yes
  vars:
    nginx_package: nginx
    nginx_state: present
  tasks:
  - name: Install nginx package
    ansible.builtin.yum:
      name: "{{ nginx_package }}"
      state: "{{ nginx_state }}"
```
Here, nginx_package and nginx-state are variables that make it easier to change values without modifying the entire playbook.

# Handlers in Playbooks
Handler are special tasks that only run when notified by another task. Handlers are often used to restart services or re-load configurations ofter changes.
Example with Handlers:
```
- name: Install and configure Nginx
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx package
      ansible.builtin.yum:
        name: nginx
        state: present
      notify:
          -restart nginx
  handlers:
    - name: restart nginx
      ansible.builtin.services:
      name: nginx
      state: restarted
```

# Playbook Example for Network Automation (Cisco Router)
Here is a more practival example: automating network configuration on Cisco routers.
```
- name: Confgure IP address on Cisco router
  hosts: routers
  become: yes
  tasks:
    - name: Configure IP address on interface GigabitEthernet0/0
      cisco.ios.ios_interface:
        name: GigabitEthernet0/0
        enabled: true
        ip_address: 192.168.1.1
        netmask: 255.255.255.0
        description: "WAN Interface"
      notify:
        - Save config

  handlers:
    - name: Save config
      cisco.ios.ios_command:
        commands:
          - copy running-config startup-config
```
## Explanation
- cisco.ios.ios_interface: An Ansible module for configuring interfaces on Cisco devices.
- cisco.ios.ios_command: A module for sending commands to Cisco devices.
- notify: Notifies the Save config handler to save the configuration.

# Running the Playbook
To execute the playbook, run the following command:
```
ansible-playbook -i inventory.ini configure_nginx.yml
```
This command tells Ansible to use the configure_nginx.yml playbook and the inventory.ini inventory file.




