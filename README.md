
# Digial Ocean user setup automation

This project automates the process of setting up a new user on a DigitalOcean droplet. By default, when creating a droplet, SSH access is only available as the root user. This project helps you automate the creation of a new user, adding them to the sudo group, and installing an SSH key for the user to access the server.

## Prerequisites

**Ansible**: You need to have Ansible installed on your local machine:

   ```
   sudo apt install ansible -y
   ```

## How It Works
This repository consists of three main files:

* **create_inventory.yml**: Creates an inventory file for Ansible.
* **playbook.yml**: Automates the user setup on the droplet (creating a user, adding them to the sudo group, setting up an SSH key, disabling root login, and restarting the SSH service).
* **inventory.ini.j2**: A Jinja2 template for generating an Ansible inventory file.

## Setup
1. Create a vars.yml File
Before running the playbooks, you need to create a vars.yml file where you will define the required variables.

Example of vars.yml:


```
user_name: "newuser"  # The username of the new user to be created
user_password: "secure_password"  # The password for the new user
root_user: "root"  # The root user (default for DigitalOcean droplets)
public_key_path: "/path/to/public_key"  # Path to the public SSH key to be installed for the new user
private_key_path: "/path/to/private_key"  # Path to the private SSH key for root (used for initial access)
droplet_ip: "your_droplet_ip"  # The IP address of the droplet

```

Note: Make sure to replace the placeholder values with your actual droplet details.

2. Create the Ansible Inventory File
To generate the inventory.ini file, run the following command:

```
ansible-playbook create_inventory.yml
```

3. Run the Playbook to Setup the User
After creating the inventory.ini file, run the playbook to configure the droplet with the new user:

```
ansible-playbook -i inventory.ini playbook.yml

```
This will:

Create the user specified in vars.yml, add the user to the sudo group, install the SSH public key for the user to allow SSH login, disable root login in the SSH configuration, and restart the SSH service to apply the changes.
