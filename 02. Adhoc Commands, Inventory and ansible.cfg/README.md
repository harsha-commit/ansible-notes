# Section 2 - Adhoc Commands, Inventory & ansible.cfg

## Ansible Components

- ansible.cfg
- Inventory / Hosts
- Tasks
- Playbooks
- Modules

## Ansible Basics

- All ansible commands start with "ansible"
- Ansible default configuration file exists at ``/etc/ansible/ansible.cfg`
- Default inventory file exists at `/etc/ansible/hosts`
- Managed nodes information should be available in inventory file

## Ansible Adhoc Commands

- one-liners designed to achieve a very specific task, they are like quick snippets
- ping
	- `ansible all -m ping`
		- Ping all the servers from the host file
		- -m refers to module, ping is the module
- command
	- `ansible all -m command -a "uptime"`
		- Execute linux commands on the servers
		- -a refers to attribute
	- `ansible all -m command -a "ls -l"`
		- Executes at the home directory of the servers
- stat
	- `ansible all -m stat -a "path=/home/ansadmin/testfile"`
		- Returns all the stats of a file, if found
- yum
	- `ansible all -m yum -a "name=git" -b `
		- Install desired packages on the servers
		- -b refers to become root (for sudo permissions)
- user
	- `ansible all -m user -a "name=test" -b`
		- Create user with specified name on the servers
- setup
	- `ansible all -m setup`
		- Gives all the information about the servers

## Ansible Inventory

- Ansible works against multiple managed nodes or hosts in your infrastructure at the same time, using a list or group of lists known as Inventory
- Hosts information can be defined in
	- Default location: `/etc/ansible/hosts`
	- Use -i option: `ansible -i my_hosts`
	- Defined in ansible.cfg file

### Manage Specific Nodes

1. In the control node, create an inventory file `vi inventory.ini`
2. Add desired IP Addresses in the inventory, that you want to manage
3. Run `ansible all -i /home/ansadmin/inventory.ini -m user -a "name=hello" -b`
4. Or you can edit the default inventory in ansible.cfg
5. And then `ansible all -m user -a "name=hello" -b`

## Basic Parameters in ansible.cfg

- inventory
	- location of the inventory file
- library
	- location of the modules directory
- module_utils
	- location of the modules' utilities directory
- remote_tmp
	- location of the temp folder for the managed node
- local_temp
	- location of the temp folder for the control node
- forks
	- batch size for parallel execution
- poll_interval
	- how often to check back on a launched asynchronous task in seconds
- sudo_user
	- user that executes when -b is specified
-  remote_port
	- port of the managed node server
- roles_path
	- location of the roles directory
- timeout
	- timeout time for SSH
- retry_files_enabled
	- a retry file is created when a playbook fails
- module_name
	- default module name

## Custom ansible.cfg

- Changes can be made and used in a configuration file which will be searched for in the following order:
	- `ANSIBLE_CONFIG` (environment variable if set)
	- `ansible.cfg` (in the current directory)     
	- `~/.ansible.cfg` (in the home directory, it is .ansible.cfg, not ansible.cfg)     
	- `/etc/ansible/ansible.cfg`
- Do not forget to give section headers for the parameters, like [defaults], [inventory], etc