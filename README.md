<h1 align="center">Offensive Kali Ansible Playbook</h1>

This Ansible Playbook automates the setup of kali machines used for both external and internal penetration tests. The Ansible Roles included in this playbook automates the downloading and installalation of additional frameworks, packages, and offensive penetration testing and red-teaming utilities for a Kali Linux machine. 

 [![Status](https://img.shields.io/badge/status-active-success.svg)]() 

## Table of Contents
+ [Description](#description)
+ [Getting Started](#getting_started)
+ [Usage](#usage)
+ [Execution](#execution)

## Description <a name = "description"></a>
This playbook contains multiple tasks embedded within the roles. The current roles included in this ansible playbook include the following:

- Common
  - apt package updates, cleanup, and installation of offensive packages
  - installation of common offensive python packages
  - installation of common git repos as well as setting up their package dependencies
- External
- Internal

## Getting Started <a name = "getting_started"></a>
There are two ways you can deploy this ansible playbook. 
- On local Kali Host
- Remote Connection from mac (or linux) to Single or Multiple Hosts *Sorry Windows*

### Kali or Linux Host
The following is required to be on the system before running this ansible playbook
- ansible

This can be installed using the following command

`sudo apt-get install ansible`

### Mac - homebrew
The following is required on your Mac before installing ansible
- Homebrew

*If you don't have homebrew, it can be downloaded using the following command*

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

With homebrew, run the following commands to install ansible

```bash
# Update brew
brew update

# Install ansible 
brew install ansible
```
---
## Usage <a name = "usage"></a>
To use this playbook, you can either run it from the kali host locally or you can deploy it remotely to a single or multiple hosts from your mac (or linux machine).

### Roles
To decide which roles you would like to do, edit the `site.yml` file.

Ex: If you are performing an external penetration test, the site file should look like this:

```yaml
# Main Playbook
---
- hosts: kali
  roles:
    - common 
    - external
```
Vice versa for internal, or even both! They can be integrated to include all tools for each portion of a test. 

### Local Execution - Kali Host
 After ansible is installed on your local kali host, clone this repo and run ansible playbook.

```bash
# clone repo, move to directory, execute playbook 
git clone https://github.com/hackedbyagirl/offensive-kali-ansible.git
cd offensive-kali-ansible
ansible-playbook -i ansible/local.ini site.yml -K
```
*By default, this repo is only made to be used with one host*

### Remote Connection - Single or Multiple Hosts
This ansible playbook can be deployed against a signle host or multiple machines at the same through a remote connection (our method will be SSH). The following will be accomplished by:
1. Create a host inventory
2. Set up SSH conenctions for each host
3. Set Hosts in site.yml
4. Set Remote User
5. Ensure SSH connection
6. Run

### Set inventory
This playbook is intented to automate a defaut offensive environment on kali hosts. In order to use this playbook efficently, it should be run against an inventory of kali hosts. This can be done by creating an inventory of hosts.


To configure the hosts inventory, open and edit the hosts.ini file to include the hosts in the following manner. This is jsut an example. 

```
[kali]
192.xx.xx.xx
10.xx.xx.xx

[kali:vars]
ansible_connection=ssh 
ansible_user=kali

```
### Set up SSH connection
To ensure proper ssh connection, remote key-based authentication must be configured before deploying the playbook. Please do the following on each host that you have listed in your inventory file:

```bash
# Generate ssh key -- if you have an id_ed25519 ssh key -- skip this step
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "testuser@ansible-hosts"

# Add ssh key to ssh-agent
ssh-add ~/.ssh/id_ed25519

# Specify specific key to SSH into a remote server
ssh-copy-id -i ~/.ssh/id_ed25519 kali@198.xx.xx.xx
```

### Configure Hosts in site.yml
This playbook sets up ansible to be ran on a local host. To change that to, edit the `site.yml` file and change 

`hosts: localhost` to `hosts: kali`


### Setting a Remote User
By default, ansible connects to all remote devices with the username you are using on the control node. If that username does not exist on the remote device, you will need to set a different username for the connection in the playbook. By default, this playbook will have the username set to `kali` in the inventory file `ansible/hosts.ini`


## Execution <a name = "execution"></a>
```bash
# clone repo, move to directory, execute playbook 
git clone https://github.com/hackedbyagirl/offensive-kali-ansible.git
cd offensive-kali-ansible

# Edit inventory file with host and configurations -- save 
vim ansible/hosts.ini

# Edit site.yml to ensure it's being deployed on kali hosts
vim site.yml

# Deploy playbook
ansible-playbook -i ansible/hosts.ini site.yml --ask-become-pass
```
