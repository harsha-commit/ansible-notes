# Section 3 - Modules and Playbook

## Ansible Modules

- A module is a reusable, standalone script that Ansible runs on your behalf, either locally or remotely
- Modules interact with local machine, an API or an remote system to perform specific tasks like
	- Creating Users
	- Installing packages
	- Updating configurations
	- Spinning up instances, etc
- Modules are the programs that perform the actual work of the tasks of the play
- Play is basically the complete execution of a Playbook
- Ansible ships with thousands of modules
- Modules are used to perform tasks

## Ansible Playbook

- A Playbook is a text file written in YAML format and is normally saved as .yml
- The playbook begins with a line consisting of three dashes as a start of document marker
- An item in a YAML list starts with a single dash followed by a space
- hosts and tasks are mandatory items in a playbook
- No dashes for parameters of a module
- Gathering facts task is by default run by a playbook
- The Playbook is executed sequentially, task by task

### Example Playbooks

```yaml
# Equivalent to ansible all -m user "name=John" -b
# vim create_user.yml
# Run this playbook by ansible-playbook create_user.yml
# Verify a playbook by ansible-playbook create_user.yml --check

---
- hosts: all
  become: true
  tasks:
  - user: name=John
```

```yaml
# Adding Metadata to improve Play and Playbook readability
# Removing Gathering Facts Task (It collects Server's Complete Info)
# Changing Parameter's format

---
- name: Creating a User
  hosts: all
  gather_facts: no
  become: true
  
  tasks:
  - name: Creating a User with name John
    user:
      name: John
```

```yaml
# vi install_package.yml
# ansible-playbook install_package.yml --check
# ansible-playbook install_package.yml

---
- name: Installing a Package
  hosts: all
  gather_facts: no
  become: true
  
  tasks:
  - name: Installing yum package
    yum:
      name: httpd
      state: installed
  - name: Starting http services
    service:
      name: httpd
      state: started
```