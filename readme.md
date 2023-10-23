# Documentation for my ansible

To run ansible use this cmd

```bash
ansible-playbook --ask-become-pass playbook-name.yml
```

state can be latest, absent
latest means to install the latest version of that software, while absent is to uninstall a software.

## Conditional (When)

To check if distribution is ubuntu before running a command

```bash
when: ansible_distribution == "ubuntu"
```

To make work in different distributions

```bash
when: ansible_distribution in ["Debian", "ubuntu"]
```

To gather facts from just one server when you have many

```bash
ansible all -m gather_facts --limit server_ipaddress
```

When you run this comand

```bash
ansible all -m gather_facts --limit server_ipaddress | grep ansible_distribution
```

<p><b>you will have this </b></p>
<p>ansible_distribution</p>
<p>ansible_distribution_file_parsed</p>
<p>ansible_distribution_file_path</p>
<p>ansible_distribution_file_variety</p>
<p>ansible_distribution_major_version</p>
<p>ansible_distribution_release</p>
<p>ansible_distribution_version</p>

<p>This will enable you to use two or more conditions</p>

```bash
when: ansible_distribution_major_version == "8.2" and ansible_distribution == "CentOS"
```

## Variables

```yaml
name:
  apt:
    name:
      - "{{ apache_package }}"
      - "{{ php_package }}"
```

Go to your inventory file and add the variables there
172.16.50.132 apache_package=apache2 php_package=php

Instead of using apt, you can use "package" this will use a compatible package of the linux distribution.

## Target Nodes in Inventory file
[web_server]
192.172.80.122

[db_server]
172.892.54.172

To use this on the playbook
```yaml
name:
  apt:
    name:
      - "{{ apache_package }}"
      - "{{ php_package }}"
```

pre_tasks is used when you want that tasks to be run before others.

## Tags
```bash
name:
  tag: apache,centos
  apt:
    name: apache2
```
To get the list of your tags use this command:
```bash
ansible-playbook --list-tags playbook-name.yml
```
To target tasks with only tag with apache:
```bash
ansible-playbook --tags apache playbook-name.yml
```
To target multiple tags use
```bash
ansible-playbook --tags "apache,php" playbook-name.yml
```

if your server username is not the same with your local machine username, then the inventory should look like this.

```bash
[all]
173.230.141.217 ansible_user=root
```
