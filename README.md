# Deploying Create React App with Ansible Playbooks

## Overview
This documentation provides a step-by-step guide on how to deploy a React application using Ansible playbooks to both a Red Hat and Ubuntu server. The process involves setting up Ansible playbooks to automate the deployment process for Create React App on different server environments.

## Prerequisites
- Basic understanding of Ansible
- Access to Red Hat and Ubuntu servers
- Create React App project configured for deployment
- Ansible installed on the control machine

## Understanding Tasks in Ansible Playbook

1. Install Node.js, npm, and Git on RedHat machine if not already installed, updating the package cache before installation.
```` bash
- name: Install Node.js and Git  
  dnf:                          
    name:                        
      - nodejs
      - npm
      - git
    state: present               
    update_cache: yes            
  when: ansible_distribution=="RedHat"  
  tags:                          
    - 1
````

2. Install Node.js, npm, and Git on Ubuntu machine if not already installed, updating the package cache before installation.
```` bash
- name: Install Node.js 16 and Git on Ubuntu machine
  apt:
    name:
      - nodejs
      - git
      - npm
    state: present
    update_cache: yes
  when: ansible_distribution=="Ubuntu"
  tags:
    - 2
````

3. This Ansible task clones a Git repository into the "/app" directory, forcefully overwriting any existing content.
```` bash
- name: Clone Git repository into the "/app" directory, forcefully overwriting any existing content.
  git:
    repo: https://github.com/harshm1225/create-react-app.git
    dest: /app
    force: yes
  tags:
    - 3
````

4. This Ansible task installs project dependencies using npm within the "/app" directory.
```` bash
- name: Install project dependencies with npm within the "/app" directory.
  command: /usr/bin/npm install
  args:
    chdir: /app
  tags:
    - 4
````

5. This Ansible task runs npm build within the "/app" directory, based on the distribution being RedHat or Ubuntu.
```` bash
- name: Run npm build within the "/app" directory, based on the distribution being RedHat or Ubuntu.
  block:
    - command: /usr/bin/npm run build
      args:
        chdir: /app
  when: ansible_distribution == "RedHat" or ansible_distribution == "Ubuntu"
  tags:
    - 5
````

6. This Ansible task installs the Apache Web Server on an Ubuntu machine using the apt package manager.
```` bash
- name: Install Apache Web Server on Ubuntu machine using the apt package manager.
  apt:
    name: apache2
    state: latest
  when: ansible_distribution=="Ubuntu"
  tags:
    - 6
````

7. This Ansible task installs the Apache Web Server on Redhat machine using the yum package manager.
```` bash
- name: Install Apache Web Server on RedHat machine using the yum package manager.
  yum:
    name: httpd
    state: latest
  when: ansible_distribution=="RedHat"
  tags:
    - 7
````

8. This task ensures Apache Web Server is enabled and started on Ubuntu machines.
```` bash
- name: Enable Apache Web Server Service on Ubuntu machine
  service:
    name: apache2
    enabled: true
    state: started
  when: ansible_distribution=="Ubuntu"
  tags:
    - 9
````

9. This task ensures Apache Web Server is enabled and started on RedHat machines.
```` bash
- name: Enable Apache Web Server Service on RedHat machine
  service:
    name: httpd
    enabled: true
    state: started
  when: ansible_distribution=="RedHat"
  tags:
    - 10
````

10. This task copies the index.html file from the /app/build/ directory to the default Apache document root (/var/www/html/) on the target machine.
```` bash
- name: Copy index.html to Default Apache Document root
  copy:
    src: /app/build/
    dest: /var/www/html/
    remote_src: yes
  tags:
    - 11
````

11. This task sets Apache file permissions on RedHat systems, ensuring the /var/www/html directory is owned by 'apache' user and group with permissions 0755. It's essential for Apache to access and serve files correctly.
```` bash
- name: Set file permissions for Apache on RedHat
  file:
    path: /var/www/html
    owner: apache
    group: apache
    mode: "0755"
  when: ansible_distribution=="RedHat"
  tags:
    - 12
````

12. This task sets file permissions for Apache on Ubuntu, ensuring that the /var/www/html directory is owned by 'www-data' user and group with permissions 0755. This setup facilitates Apache's ability to access and serve files correctly.
```` bash
- name: Set file permissions for Apache on Ubuntu
  file:
    path: /var/www/html
    owner: www-data
    group: www-data
    mode: "0755"
  when: ansible_distribution=="Ubuntu"
  tags:
    - 13
````

13. This task removes SELinux permissions and restores them to default for RedHat systems. It utilizes the restorecon command to recursively restore context on the /var/www/html directory.
```` bash
- name: Remove SELinux permissions and set to default for RedHat
  command: restorecon -Rv /var/www/html
  when: ansible_distribution=="RedHat"
  tags:
    - 14
````

14. This task removes SELinux permissions and sets them to default for Ubuntu systems by using the chcon command. It assigns the SELinux context system_u:object_r:default_t to the /var/www/html directory.
```` bash
- name: Remove SELinux permissions and set to default for Ubuntu
  command: "chcon system_u:object_r:default_t /var/www/html"
  when: ansible_distribution=="Ubuntu"
  tags:
    - 15
````

15. This task restarts the Apache service on RedHat systems using the service module. It ensures the service named 'httpd' is restarted.
```` bash
- name: Restart Apache service on RedHat
  service:
    name: httpd
    state: restarted
  when: ansible_distribution=="RedHat"
  tags:
    - 16
````



































