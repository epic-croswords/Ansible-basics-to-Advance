## Ansible: (DAY 01)

Overview

Learn what Ansible is, its purpose, configuration management, installation, advantages and disadvantages, and a comparison with Puppet. We'll also cover creating YAML files, AD-HOC commands, YAML indentations, playbooks, Ansible roles, galaxy commands, advanced concepts, securing credentials, and event handling.
Why Use Ansible

Ansible is a configuration management tool for automating tasks across multiple machines, reducing manual overhead. It simplifies repetitive tasks and ensures consistency.
Configuration Management

Before configuration management, sysadmins and developers manually configured systems. Ansible automates these tasks, converting scripts and commands into idempotent playbooks.
Advantages of Ansible

    Pushes changes to all servers without requiring additional software.
    Ensures the same configuration is maintained, avoiding unintended consequences of repeated shell scripts.

Idempotence

Ensures operations produce the same result, whether run once or multiple times, maintaining consistent configurations.
Installing Ansible

Requires Python. Installation steps:
Fedora/RHEL/CentOS

sh

yum install epel-release
yum install ansible -y

Debian/Ubuntu

sh

sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible -y

Verify installation:

sh

ansible --version

Creating a Basic Inventory File

Ansible uses inventory files to manage servers. Create a file at /etc/ansible/hosts:

ini

[example] # group of hostname/IP
www.example.com

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

This structure provides a clear and concise overview of ad hoc commands, making it easy for users to understand and implement them.
