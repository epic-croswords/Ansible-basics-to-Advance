## Ansible (DAY 02):

### Ansible playbook:
> like any other configuration management tool, ansible uses a metaphor to describe its configuration files. They called as ‘playbooks’ & they contain list of tasks/plays that run on hosts machines. 
Those playbooks written in YAML format it is simple human-readable format syntax to define configuration, in heigh level playbooks can include other playbooks, metadata, modules, variables, loops & conditions etc.  
### NOTE: you can keep ansible playbook extension ".yml" or ".yaml" both thigs are same

### Example:
> What attracts most DevOps personnel to Ansible is the fact that it is easy to convert shell scripts (or one-off shell commands) directly into Ansible plays. 
Consider the following script, which installs Apache on a RHEL/CentOS server:

```
#!/bin/bash
#update repo 
yum update -y 

#installing http (Apache) package 
yum install httpd -y
yum install curl -y

#Move any default pages of httpd present inside that folder
mv /var/www/html/* /tmp

#Add index.html look for website
echo “This is my public Ip $(curl https://ifconfig.me/)” >> /var/www/html/index.html

# enable & start the services 	
systemc	tl enable httpd
systemctl start httpd
```

to run the shell script (in this case, a file named “shell.sh” with the contents as above you would call it directly from the command line:
```
	$ ./shell.sh
```

when you running this script as a bootstrap script its okay to use this but when running the same script across bunch of hosts it becomes quite hectic. 

### NOTE: make sure keep indentation correct (spaces, tabs) etc. otherwise you will face syntax errors.

### Ansible plays:

```

---

- name: installing Apache server and configure.
  hosts: all
  become: True
  
  tasks:
	- name: install dependancy
	  command: >
		yum install httpd -y
		yum install curl -y
		
	- name: move default html default files into /tmp
	  copy:
		src: /var/www/html/*
		dest: /tmp
		follow: no
		
	- name: create index.html page accourding to our need & start the services
	  command: >
		echo “This is my public Ip $(curl https://ifconfig.me/)” >> /var/www/html/index.html
		systemctl enable httpd
		systemctl start httpd
		
```

to run ansible playbooks ( in this case i have provided filename as "playbook.yml" ) you would calling using ansible-playbook command:
```
	$ ansible-playbook playbook.yml	  
```

Now we’re getting somewhere. Let me walk you through this simple playbook:

1. The first line, ---, is how we mark this document as using YAML syntax (like using <html> at the top of an HTML document, or <?php at the top of a block
of PHP code).

2. On the second line -- hosts: all | your group hosts that contains hostnames or ip addresses.

3. The third line, become: yes tells Ansible to run all the commands through sudo,
so the commands will be run as the root user.

4. The fifth line, tasks:, tells Ansible that what follows is a list of tasks to run as
part of this playbook.

5. The first task begins with name: Install dependancy.. name is not a module that
does something to your server; rather, it’s a way of giving a human-readable
description to the task that follows. Seeing “Install dependancy” is more relevant
than seeing “yum name=httpd state=present”… but if you drop the name line
completely, that won’t cause any problem.

We use the yum module to install Apache. Instead of the command yum -y
install httpd httpd-devel, we can describe to Ansible exactly what we
want. Ansible will take the list of packages we provide. We tell yum to
make sure the packages are installed with state: present, but we could
also use state: latest to ensure the latest version is installed, or state:
absent to make sure the packages are not installed.


### Command: the grater sign (>)

> The greater-than sign (>) immediately following multiple lines of commands 


### Limiting playbooks to particular hosts and groups

> You can limit a playbook to specific groups or individual hosts by changing the
hosts: definition. The value can be set to all hosts, a group of hosts defined in your
inventory, multiple groups of hosts (e.g. webservers,dbservers), individual hosts
(e.g. atl.example.com), or a mixture of hosts. You can even do wild card matches,
like *.example.com, to match all subdomains of a top-level domain.

```
	$ ansible-playbook playbook.yml --limit all
```

If you want to see a list of hosts that would be affected by your playbook before you
actually run it, use --list-hosts:

```
	$ ansible-playbook playbook.yml --list-hosts
```

```
Running this should give output like:
playbook: playbook.yml

play #1 (all): host count=4
	127.0.0.1
	192.168.24.2
	foo.example.com
	bar.example.com

```

### Setting user and sudo options with ansible-playbook

> If no remote_user is defined alongside the hosts in a playbook, Ansible assumes
you’ll connect as the user defined in your inventory file for a particular host, and
then will fall back to your local user account name. You can explicitly define a remote
user to use for remote plays using the --user (-u) option:

```
	$ ansible-playbook playbook.yml --user=root
```

#### option 1
> In some situations, you will need to pass along your sudo password to the remote
server to perform commands via sudo. In these situations, you’ll need use the
--ask-become-pass (-K) option.

#### option 1
> You can also explicitly force all tasks in a playbook
to use sudo with --become (-b). Finally, you can define the sudo user for tasks run
via sudo (the default is root) with the --become-user (-U) option.

### command:
```
	$ ansible-playbook playbook.yml --become --become-user=janedoe \
	  --ask-become-pass
```

