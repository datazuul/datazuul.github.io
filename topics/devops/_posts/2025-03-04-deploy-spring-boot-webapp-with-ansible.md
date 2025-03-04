---
layout: post
title: Deploy a Spring Boot Webapp to a VM with Ansible
author: Ralf Eichinger
toc: true
---

In this Post we describe how to deploy a Spring Boot Webapp to a remote VM using Ansible.
Make sure you already [installed Ansible]({% post_url topics/devops/2025-03-03-ansible %}) and tried your first playbook.

We will use Ansible directory layout as described in our post ["How to structure Ansible directory layout?"]({% post_url topics/devops/2025-03-04-how-to-structure-ansible-directory-layout %})

# Create Ansible Playbook

## For deploying a local JAR

To keep things simple at first we create a playbook for deploying a webapp from local storage.

We assume that you compiled and packaged your Spring Boot Webapp as JAR with `mvn clean install` if you're using Maven or `./gradlew build` if you're using Gradle.

The packaged webapp's JAR will be placed in `~/.m2/repository` (Maven) or in the `build/libs` directory of your project (Gradle).

Our example uses Maven installed artifact under `~/.m2/repository`.

Create a new file `playbooks/deploy-my-webapp.yaml` and add the following content:

{% raw %}
```yaml
- name: Deploy my Spring Boot Webapp
  hosts: managed_host
  become: true
  vars:
    app_name: spring-boot-app
    app_version: 1.0.0
    app_jar: "{{ app_name }}-{{ app_version }}.jar"
    app_user: springboot
    app_group: webapps
    app_server_port: 10000
    app_management_server_port: 10001
    deploy_dir: /opt/{{ app_name }}
    src_dir: ~/.m2/repository/<apps_groupId_path_see_pom.xml>/{{ app_name }}/{{ app_version }}

  tasks:
    - name: Install Java Runtime Environment
      apt:
        name: default-jre
        state: present

    - name: Create group "{{ app_group }}"
      group:
        name: "{{ app_group }}"
        state: present

    - name: Create user "{{ app_user }}"
      user:
        name: "{{ app_user }}"
        state: present

    - name: Add "{{ app_user }}" to "{{ app_group }}"
      user:
        name: "{{ app_user }}"
        groups: "{{ app_group }}"
        append: yes

    - name: Create deployment directory
      file:
        path: "{{ deploy_dir }}"
        state: directory
        owner: "{{ app_user }}"
        group: "{{ app_group }}"

    - name: Create directory containing logs (location same as configured in logback-spring.xml)
      file:
        path: "{{ deploy_dir }}/logs"
        state: directory
        owner: "{{ app_user }}"
        group: "{{ app_group }}"
        mode: '0755'

    - name: Copy JAR file to managed host
      copy:
        src: "{{ src_dir }}/{{ app_jar }}"
        dest: "{{ deploy_dir }}/{{ app_jar }}"
        owner: "{{ app_user }}"
        group: "{{ app_group }}"

    - name: Create systemd service file
      template:
        src: templates/springboot.service.j2
        dest: /etc/systemd/system/{{ app_name }}.service
        owner: root
        group: root
        mode: '0644'

    - name: Reload systemd manager configuration
      ansible.builtin.command: systemctl daemon-reload
      when: not ansible_check_mode
      
    - name: Start and enable Spring Boot service
      systemd:
        name: "{{ app_name }}.service"
        state: started
        enabled: yes
      when: not ansible_check_mode
```
{% endraw %}

# Create a Systemd Service Template

The playbook references the template `templates/springboot.service.j2`.

Create `templates/springboot.service.j2` for configuring Linux systemd service:

{% raw %}
```ini
[Unit]
Description={{ app_name }}
After=syslog.target

[Service]
User={{ app_user }}
Group={{ app_group }}
ExecStart=/usr/bin/java -jar {{ deploy_dir }}/{{ app_jar }} --spring.profiles.active={{ spring_profile }}  --server.port={{ app_server_port }} --management.server.port={{ app_management_server_port }}
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```
{% endraw %}

# Set `spring_profile` variable

The template references variables from the playbook vars section and additionally `spring_profile` variable we have not defined, yet.

As the `spring.profiles.active` property value varies from environment to environment ("DEV", "STG", "PROD") we configure the var `spring_profile` in our production inventory (and the other inventories accordingly) for group "all".

File `inventories/production/group_vars/all.yml`:

```yml
spring_profile: PROD
```

# Check Ansible Playbook

Before running it against production (or preferable development environment) let's do a "dry run" for just simulating what would happen using option `--check`:

```sh
$ ansible-playbook -i inventories/production/inventory.yml -l web1 playbooks/deploy-my-webapp.yaml --check
PLAY [Deploy Spring Boot Webapp] ***********************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************************************************************************
ok: [web1]

TASK [Install Java Runtime Environment] *****************************************************************************************************************************************************************************************************************
ok: [web1]

TASK [Create group "webapps"] ***************************************************************************************************************************************************************************************************************************
changed: [web1]

TASK [Create user "springboot"] *************************************************************************************************************************************************************************************************************************
changed: [web1]

TASK [Add "springboot" to "webapps"] ********************************************************************************************************************************************************************************************************************
changed: [web1]

TASK [Create user "springboot" with group "webapps"] ****************************************************************************************************************************************************************************************************
changed: [web1]

TASK [Create deployment directory] **********************************************************************************************************************************************************************************************************************
[WARNING]: failed to look up user springboot. Create user up to this point in real play
[WARNING]: failed to look up group webapps. Create group up to this point in real play
changed: [web1]

TASK [Create directory containing logs (location same as configured in logback-spring.xml)] *************************************************************************************************************************************************************
changed: [web1]

TASK [Copy JAR file to managed host] ********************************************************************************************************************************************************************************************************************
changed: [web1]

TASK [Create systemd service file] **********************************************************************************************************************************************************************************************************************
changed: [web1]

TASK [Reload systemd manager configuration] *************************************************************************************************************************************************************************************************************
changed: [web1]

TASK [Start and enable Spring Boot service] *************************************************************************************************************************************************************************************************************
skipping: [web1]

PLAY RECAP **********************************************************************************************************************************************************************************************************************************************
web1                 : ok=8    changed=6    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```

# Run Ansible Playbook

Run the playbook with the following command to deploy the webapp (limited) to host `web1` in production:

```sh
$ ansible-playbook -i inventories/production/inventory.yml -l web1 playbooks/deploy-my-webapp.yaml
```

# Check Webservice

You now can login to the host `web1` and check running webservice using `wget`:

```sh
$ wget localhost:10000
--2025-03-04 15:30:32--  http://localhost:10000/
Resolving localhost (localhost)... ::1, 127.0.0.1
Connecting to localhost (localhost)|::1|:10000... connected.
HTTP request sent, awaiting response... 200 
Length: 404 [text/html]
Saving to: ‘index.html’

index.html                                           100%[===================================================================================================================>]     404  --.-KB/s    in 0s      

2025-03-04 15:30:32 (79.0 MB/s) - ‘index.html’ saved [404/404]
```

Assuming you configured Spring Boot Actuator to reside under path `/monitoring`:

```yml
management:
  endpoints:
    web:
      base-path: '/monitoring'
```

You can get health status calling url `http://localhost:10001/monitoring/health`.

```json
{"status":"UP"}
```

Your webservice is up and running!

---

References:

* <https://docs.ansible.com/ansible/latest/getting_started/get_started_playbook.html>
* <https://docs.ansible.com/ansible/latest/playbook_guide/index.html>
* <https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_templating.html>