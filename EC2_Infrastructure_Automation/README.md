<div align="center">

# Infrastructure Automation with Ansible
This project automates the provisioning, configuration, and deployment of web servers across multiple `EC2` instances using `Ansible`. It includes setting up necessary tools like `Git`, installing either `Apache` or `Nginx` as the web server

All specific values (such as `AWS credentials`, `security group names`, and `subnet IDs`) have been removed from this documentation for safety and security purposes. Replace placeholders with actual values in your environment.


 ![Ansible structure]()

 ------------

 ![Static Badge](https://img.shields.io/badge/Table%20%20%20%20%20%20%20%20%20%20%20of%20%20%20%20%20%20%20%20%20%20Contents-blue?style=for-the-badge&logoColor=darkviolet)

**| [Overview](#overview) | [Setup and Configuration](#setup-and-configuration) | [Playbooks](#playbooks) | [Templates](#templates) | [Running the Project](#running-the-project) |**

------------

</div>

## Overview

This project includes four main Ansible playbooks:
1. Provision EC2 Instances: Sets up EC2 instances on AWS.
2. System Setup: Installs essential dependencies and prepares the server environment.
3. Git Installation and Configuration: Installs Git and applies global configuration settings.
4. Web Server Installation and Configuration: Installs either Apache or Nginx on each EC2 instance, based on your preference.

## Setup and Configuration
### Prerequisites

- AWS Access: Ensure your AWS Access Key and Secret Key are available and stored securely. It’s recommended to encrypt these using Ansible Vault.
- Ansible Installation: Install Ansible on your control node.
- AWS CLI and Boto3: Required for interacting with AWS.

### Encrypt AWS Credentials with Ansible Vault
To secure sensitive information, I've used Ansible Vault. Encrypt `group_vars/all.yml` with your AWS credentials and other sensitive data:
```bash
ansible-vault encrypt group_vars/all.yml
```
To edit the file later:
```bash
ansible-vault edit group_vars/all.yml
```

### Playbooks
- Provision EC2 Instances (`provision.yml`)
This playbook provisions EC2 instances using the Ansible AWS module. Replace placeholders like your-key-pair-name, ami-xxxxxxx, and security_groups with actual values in your environment.
Main tasks:
1. Create EC2 instances in the specified AWS region.
2. Add instances to a dynamic inventory group for subsequent tasks.
3. Wait for SSH access to each instance.

- System Setup (`setup.yml`)
This playbook updates the system, installs dependencies (such as curl and unzip), and configures firewall rules if necessary.

- Git Installation and Configuration (`git.yml`)
This playbook installs Git on each EC2 instance and sets global configuration settings for Git username and email.
Main tasks:
1. Install Git.
2. Set global Git username and email.
3. Verify installation and display Git version.

- Web Server Installation and Configuration (`web_server.yml`)
This playbook installs and configures either Apache or Nginx based on the web_server variable set in group_vars/all.yml.
Main tasks:
1. Install Apache or Nginx based on the variable value.
2. Apply the respective configuration template (either apache2.conf.j2 or nginx.conf.j2).
3. Start and enable the selected web server on boot.

## Templates
- Apache Configuration (`apache2.conf.j2`)
This configuration template for Apache sets up a basic web server on port 80 with essential security headers enabled and logging paths configured.

- Nginx Configuration (`nginx.conf.j2`)
This configuration template for Nginx sets up a basic server block on port 80 with security headers enabled and logging paths configured.

## Running the Project

1. Run the Main Playbook: Use the following command to start the entire setup process. Ansible will prompt for the Vault password if `group_vars/all.yml` is encrypted.
```bash
ansible-playbook playbooks/site.yml --ask-vault-pass
```
2. Verify Deployment: Once all tasks complete, verify that the selected web server is running on each instance by accessing the instance's public IP.
