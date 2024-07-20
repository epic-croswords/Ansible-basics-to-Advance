## Ansible: (DAY 01)

Let’s look what is ansible what is term called configuration management, why do we need ansible & its purpose, how to install & configure ansible, how ansible work what its advantages and disadvantages will look. Puppet vs Ansible, in the few next chapters will see how to create own YAML files, AD-HOC commands, YAML indentations, playbooks, ansible roles, galaxy commands & much more will see advance concepts to secure credentials automating some things, ansible event handling etc.

### Why do we use ansible:
>	Ansible is a configuration management tool that are used to perform automation on several hardware or machines simultaneously, it used to reduce the manual overhead after resource created thru ansible automations we can configure that resource according to our need, or when need to change in the configuration of every Linux server that you have every time you need to login via SSH to modify config files & install/uninstall some libraries etc. with ansible it becomes quite easy to do those things because of with it becomes extremely useful, easy to manage to config with all servers, to perform repeated actions etc. those things are managed by ansible.

### What is configuration management:
>	Before the configuration management comes in the picture usually sysadmins & developers needs read and configure every configuration manually by hand it becomes quite difficult to recreate those things from scratch every time. They found out bash scripting can be useful for them to simplify those things in scripted manner and it becomes quite easy to do those activities, here comes ansible One of Ansible’s greatest strengths is its ability to run regular shell commands verbatim, so you can take existing scripts and commands and work on converting them into idempotent playbooks as time allows. For someone (like me) who was comfortable with the command line, but never became proficient in more compli- cated tools like Puppet or Chef (which both required at least a slight understanding of Ruby and/or a custom language just to get started), Ansible was a breath of fresh air.  

### Advantage of Ansible:
Ansible works by pushing changes out to all your servers (by default), and requires no extra software to be installed on your servers (thus no extra memory footprint, and no extra daemon to manage), unlike most other configuration management tools.

### Idempotence:
Its ability to run an operation which produces the same result whether run once or multiple times. 
An important feature of a configuration management tool is its ability to ensure the same configuration is maintained whether you run it once or a thousand times. Many shell scripts have unintended consequences if run more than once, but Ansible deploys the same configuration to a server over and over again without making any changes after the first deployment.

### Installing Ansible:
Ansible requires only one dependency which is python. (Best way to install thru OS repository)
The easiest way to install Ansible on a Fedora-like system is to use the official yum package. If you’re running Red Hat Enterprise Linux (RHEL) or CentOS, you need to install EPEL’s RPM before you install Ansible (see the info section below for instructions):
-	yum install epel-release
-	Yum install Ansible* -y


The easiest way to install Ansible on a Debian or Ubuntu system is to use the official apt package.
-	sudo apt-add-repository -y ppa:ansible/ansible
-	sudo apt-get update
-	sudo apt-get install ansible -y


verify is it installed or not:
-	ansible –version


and done.

### Creating basic inventory file:
Ansible uses inventory to mange list of servers to communicate with your servers. Like hosts file (/etc/hosts) that matches servers (IP address or domain names) to groups. Inventory file can also send vars to authenticate with servers or also it can able to store variable = values lines. Just create simple file with with one server. Create file at /etc/ansible/hosts (the default location for the ansibles inventory file)

Edit this hosts file with nano, vim or whatever editor you need to edit file as sudo or root.
```
[example] # group of hostname/IP
www.example.com
```


Here's a GitHub-friendly syntax for an introduction to ad hoc commands in Ansible:


---

## Ad Hoc Commands in Ansible

Ad hoc commands allow you to execute simple tasks on your managed nodes directly from the command line without needing to write a playbook. They are ideal for quick, one-time tasks.

### Basic Syntax
```sh
ansible [host-pattern] -m [module] -a "[module options]"
```

### Examples

#### Ping All Hosts
```sh
ansible all -m ping
```

#### Check Disk Usage
```sh
ansible all -m shell -a 'df -h'
```

#### Restart a Service
```sh
ansible all -m service -a 'name=httpd state=restarted'
```

### Explanation
- `ansible` : The command to run Ansible.
- `[host-pattern]` : The group of hosts or specific hosts to run the command against.
- `-m [module]` : The Ansible module to use (e.g., `ping`, `shell`, `service`).
- `-a "[module options]"` : The arguments for the module.

### Common Modules
- `ping`: Checks the availability of hosts.
- `command`: Runs commands on remote nodes.
- `shell`: Runs shell commands on remote nodes.
- `copy`: Copies files to remote nodes.
- `service`: Manages services on remote nodes.

---

Config File (ansible.cfg)

The ansible.cfg file is used to customize Ansible configuration. Create this file in your project directory to override default settings.
Example ansible.cfg

```
[defaults]
inventory = ./hosts
remote_user = your_user
host_key_checking = False
```

Hosts File

The hosts file lists the managed nodes and groups them for easy reference in playbooks.
Example hosts File

```

[webservers]
webserver1.example.com
webserver2.example.com

[databases]
db1.example.com
db2.example.com
```

This structure provides a clear and concise overview of ad hoc commands, making it easy for users to understand and implement them.
