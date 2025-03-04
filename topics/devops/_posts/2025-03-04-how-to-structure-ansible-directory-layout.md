---
layout: post
title: How to structure Ansible directory layout?
author: Ralf Eichinger
toc: true
---

An organized directory structure can make a big difference in maintaining your Ansible projects.

# Ansible directory layout

Here's a commonly used layout (under an example directory `ansible`, you name it like you prefer ...) that balances modularity, reusability, and readability:

```sh
ansible
├── ansible.cfg
├── inventories
│   ├── development
│   │   ├── inventory.yml
|   |   ├── group_vars
|   |   └── host_vars
│   ├── production
│   │   ├── inventory.yml
|   |   ├── group_vars
|   |   │   ├── all
|   |   │   ├── webservers
|   |   │   └── dbservers
|   |   └── host_vars
|   |       └── hostname1
│   └── staging
│   │   ├── inventory.yml
|   |   ├── group_vars
|   |   └── host_vars
├── playbooks
│   ├── site.yml
│   ├── webservers.yml
│   └── dbservers.yml
├── roles
│   ├── common
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── common_template.j2
│   ├── webserver
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── webserver_template.j2
│   └── database
│       ├── tasks
│       │   └── main.yml
│       └── templates
│           └── db_template.j2
└── files
    ├── ...
```

Here's a breakdown of each component:

* `ansible.cfg`: The configuration file for Ansible.
* `inventories`: Directory for different environment inventories (e.g. development, staging, production).
* `group_vars`: Directory for variables that apply to groups of hosts.
* `host_vars`: Directory for variables that apply to individual hosts. Ansible automatically loads the variables from the `host_vars` directory for each host in your inventory.
* `playbooks`: Directory for playbooks that define the tasks to be executed.
* `roles`: Directory for roles, which are reusable and modular units that can contain tasks, handlers, files, templates, and variables.
* `templates`: Each role can have its own templates directory to store Jinja templates. This directory will contain `.j2` files that can be rendered with variables when the playbook runs.
* `files`: Directory for files that need to be copied to remote hosts.

This structure helps to keep your files organized and makes it easier to manage and scale your infrastructure as code (IaC).

# Configure groups and hosts in inventory

> "Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations. Using an inventory file, Ansible can manage a large number of hosts with a single command."

## Add groups and hosts

Let's create an inventory file for all production hosts we want to manage using ansible. We prefer `yml` format over `ini` format:

```sh
$ nano inventories/production/inventory.yml
all:
  hosts:
    web1:
      ansible_host: 192.0.2.100
    web2:
      ansible_host: 192.0.2.110
```

* First level: "all" is the group name (as we do not specify special groups, yet)
* Third level: "web2" is an unique name describing the host
* Fourth level: "ansible_host" variable: IP-address or fully qualified domain name (FQDN) of managed host

> "Even if you do not define any groups in your inventory file, Ansible creates two default groups: all and ungrouped. The all group contains every host. The ungrouped group contains all hosts that don’t have another group aside from all. Every host will always belong to at least 2 groups (all and ungrouped or all and some other group)."

## Move host variables to `host_vars`

To separate configuration of hosts even further, we can move host specific vars to a host specific file under `host_vars` directory.

Example: Move variable `ansible_host` for host `web1` from `inventories/production/inventory.yml` to `inventories/production/host_vars/web1.yml`.

File `inventories/production/host_vars/web1.yml`:

```yml
ansible_host: 192.0.2.100
```

Do the same for host `web2`.

After moving all variables into specific host files, the final inventory just lists hosts in groups.

File `inventories/production/inventory.yml`:

```yml
all:
  hosts:
    web1:
    web2:
```

# Use host variables in a task or template

In your playbook or role, you can reference the host variables directly in your tasks or templates. Ansible automatically loads the variables based on the hostname.

Example `inventories/production/host_vars/web1.yml`:

```yml
ansible_host: 192.0.2.100
app_port: 8080
app_env: production
```

Example Jinja Template `playbooks/templates/app_config.j2`:

{% raw %}
```txt
server {
    listen {{ app_port }};
    server_name localhost;

    location / {
        proxy_pass http://localhost:{{ app_port }};
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
{% endraw %}

Example task in a playbook:

{% raw %}
```yml
- name: Deploy web application
  hosts: web1
  tasks:
    - name: Ensure application is running on the correct port
      template:
        src: templates/app_config.j2
        dest: /etc/myapp/config.conf
      notify: Restart app

    - name: Check application environment
      debug:
        msg: "The application environment is {{ app_env }}"
```
{% endraw %}

In this example, the task will ensure that the application uses the port and environment specified in the `inventories/production/host_vars/web1.yml` file. The debug task will output the application environment variable for verification.

# Roles

For further abstraction you could implement [roles](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html).

# Usage

With the directory layout in place we could do a ping to our production servers like this:

```sh
$ cd ansible
$ ansible -i inventories/production/inventory.yml all -m ping
web1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
web2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/local/bin/python3.13"
    },
    "changed": false,
    "ping": "pong"
}
```

---

References:

* <https://docs.ansible.com/ansible/latest/getting_started/get_started_inventory.html>
* <https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html>
* <https://docs.ansible.com/ansible/latest/tips_tricks/ansible_tips_tricks.html>
* <https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html>
* <https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html>
