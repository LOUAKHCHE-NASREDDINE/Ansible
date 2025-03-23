# Ansible Nginx Deployment

This repository contains an Ansible playbook to deploy Nginx with IP-based access restrictions.

## Structure
- `inventory/hostnginx`: Inventory file defining the target hosts (`vm1` and `vm2`).
- `templates/nginx.conf.j2`: Jinja2 template for the Nginx configuration.
- `nginx.yaml`: Playbook to install and configure Nginx.

## Usage
1. Clone the repository:
   ```bash
   git clone https://github.com/LOUAKHCHE-NASREDDINE/Ansible.git
   cd Ansible

