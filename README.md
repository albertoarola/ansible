
# Digial Ocean user setup automation

This project automates the process of setting up a new user on a DigitalOcean droplet. By default, when creating a droplet, SSH access is only available as the root user. This project helps you automate the creation of a new user, adding it to the sudo group, installing an SSH key for the user to access the server and disabling the SSH root login. Additionally this project automates the SSH hardnening for Ubuntu 22.04 LTS according to this guide: https://www.sshaudit.com/hardening_guides.html#ubuntu_22_04_lts

## Prerequisites

**Ansible**: You need to have Ansible installed on your local machine:

   ```
   sudo apt install ansible -y
   ```

## How It Works
This repository consists of three main files:

* **create_inventory.yml**: Creates an inventory file for Ansible.
* **playbook.yml**: Automates the user setup on the droplet (creating a user, adding it to the sudo group, setting up an SSH key, disabling SSH root login, and restarting the SSH service).
* **inventory.ini.j2**: A Jinja2 template for generating an Ansible inventory file.
* * **ssh_hardening.yml**: Automates the SSH hardening for Ubuntu 22.04 LTS

## Setup
1. Create a new droplet on Digital Ocean and retrieve IP pubblc address

2. Clone the repository
```
git clone https://github.com/albertoarola/ansible.git
cd ansible
```
3. Create a **vars.yml** File
Before running the playbooks, you need to create a **vars.yml** file where you will define the required variables in this way:
```
#vars.yml

user_name: "newuser"  # The username of the new user to be created
user_password: "secure_password"  # The password for the new user
root_user: "root"  # The root user (default for DigitalOcean droplets)
public_key_path: "/path/to/public_key"  # Path to the public SSH key to be installed for the new user
private_key_path: "/path/to/private_key"  # Path to the private SSH key for root (used for initial access)
droplet_ip: "your_droplet_ip"  # The IP address of the droplet
```

Note: Make sure to replace the placeholder values with your actual droplet details.

4. Create the Ansible Inventory File called **inventory.ini** by executing the following command
```
ansible-playbook create_inventory.yml
```

5. Run the **create_user.yml** Playbook to Setup the User
```
ansible-playbook -i inventory.ini create_user.yml
```
6. Run the **ssh_hardening.yml** Playbook to hardening SSH
```
ansible-playbook -i inventori.ini ssh_hardening.yml --ask-become-pass
```
Note: You will be asked to enter the new user's password so it can run some commands as root on the droplet. After the whole procedure in your **.ssh** directory remember to erase the old keys of the remote host in the **known_hosts** and **known_hosts.old** files

## Next times
The next times you need to create the user for a new droplet and hardening the SSH protocol the only thing you have to do is to change the IP address in your **vars.yml** with the new droplet IP and execute the three ```ansible-playbook``` commands
