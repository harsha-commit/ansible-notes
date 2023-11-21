# Section 1 - Introduction and Lab Setup
## Configuration Management

- process for maintaining computer systems, servers and software in a desired, consistent state
- way to make sure that a system performs as it's expected to, as changes are made over time
- the main purpose of Configuration Management is to implement Infrastructure as a Code

## Why Configuration Management ?

- Automation
	- Removing Manual Intervention
- Provisioning 
	- Creating new IT resources (VMs, disks, etc)
- Application Deployment
	- Ability to deploy artifacts
	- Not the primary responsibility
- Orchestration 
	- Maintaining Environment Consistency
- Uptime and Site Reliability 
	- 70% of outrages are caused due to changes in configurations
	- Changes can be reverted in case of failure

### Note

- Provisioning is the act of getting a device ready for a user
- Deployment is the whole process of getting a device to a user

## Configuration Management Tools

- Chef
- Ansible
- Salt Stack
- Puppet, etc

## What is Ansible ?

- radically simple open source IT automation engine, written in Python
- Advantages
	- Simple
		- Human readable, YAML
		- No special code skills
		- Tasks executed in order
	- Powerful
		- Configuration
		- App Deployment
		- Provisioning
		- Orchestration
	- Agentless
		- Uses OpenSSH
		- Secure
	- Efficient
	- Open Source
	- Flexible

![How Ansible Works](https://github.com/harsha-commit/ansible-notes/blob/main/Screenshots/how-ansible-works.png)

## Ansible Terminology

- Control Node
	- Any Linux machine with Ansible installed
- Managed Nodes
	- The Network devices (servers) you manage with Ansible
- Inventory
	- A list of managed nodes
	- Also called as hostfile
- Modules
	- The units of code Ansible executes
	- Each module has a particular functionality
	- Analogous to commands in Linux (actually C code)
- Tasks
	- The units of action in Ansible
- Playbooks
	- Ordered list of tasks

## Lab Setup

### Setup Control Node (Ansible Server)

1. Setup Amazon Linux EC2 Instance
2. Setup hostname 
	- `hostname controlnode` or change name using `vi /etc/hostname`
3. Create ansadmin user
	- `sudo su -`
	- `useradd ansadmin`
	- `passwd ansadmin`
4. Add user to sudoers file
	-  Edit `vi /etc/sudoers` or `visudo`
	- Add `ansadmin ALL=(ALL) NOPASSWD: ALL`
	- Enable password authentication `vi /etc/ssh/sshd_config` and `service sshd reload` 
	- Switch user `su - ansadmin`
	- Login by `ssh ansadmin@<IP_ADDR_OF_ANSIBLE_SERVER>`
5. Generate SSH keys for ansadmin (Public and Private)
	- `ssh-keygen`
6. Install Ansible
	- `sudo yum install python` 
	- `sudo yum install ansible`

### Setup Managed Nodes

1. Setup EC2 Instance
2. Setup hostname
3. Create ansadmin user
4. Add user to sudoers file
5. Enable password authentication

### Add Managed Nodes to Ansible

1. Copy public ssh keys on to managed nodes
	- In managed nodes, copy the IP Address by `ip addr` (this is Private IP address)
	- In Control node, `ssh-copy-id ansadmin@<IP_ADDR_OF_MANAGED_NODE>`
	- This asks the password of Managed node
	- Because all nodes are in the same VPC, we can use Private IP Address
2. Add server to inventory file
	- `cd /etc/ansible`
	- `touch hosts` and `chmod 777 hosts`
	- Add Managed Nodes IP Addressed to hosts `vi hosts`
	- Verify `ansible all -m ping`

### Note

- Public Keys are to be placed in Authorized Keys file
- Keys of a sever are stored in .ssh directory
- For any system with authorized key, we can log in using corresponding Private Key