# RPI Cluster Repo

Ansible Playbook (and hosts config) for installing Kubernetes on 4 Raspberry Pi's (1x Control Plane Node and 3x Worker Nodes)
Will work on Ubuntu 22.4

Credits : [This pluralsight course](https://app.pluralsight.com/library/courses/kubernetes-installation-configuration-fundamentals/table-of-contents) 


---
# To Install - see [Install.md](Install.md)
---

--- 
## Initial connectivity to Host(s) check

### Use Ansible to 'Ping' all Hosts (using -i hosts.ini)
#### - `ansible -i hosts.ini all -m ping`

### If you have setup the hosts file location in an ['ansible.cfg'](ansible.cfg) - you can omit the `-i hosts.ini`
#### - `ansible all -m ping`

---

## Update 
#### - `ansible-playbook playbook-update-and-upgrade-apt-packages.yml -K`

---

## Reboot Host(s)
#### - `ansible all -b -a "/sbin/shutdown -r now" -K`

---

## Install Chrony (Time Sync)
#### - `ansible-playbook playbook-install-and-run-chrony.yml -K`

---

## Ansible Galaxy - Prereq's
#### - `ansible-galaxy collection list`

#### - `ansible-galaxy collection install community.general` 
#### --- [community.general.modprobe](https://docs.ansible.com/ansible/latest/collections/community/general/modprobe_module.html)
---

## To target an 'Ansible Group' (e.g. cps)
#### - `ansible-playbook playbook-01-package-installation.yml --limit cps`