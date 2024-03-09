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
    - name: Clone Git repository
      git:
        repo: https://github.com/harshm1225/create-react-app.git
        dest: /app
        force: yes
      tags:
        - 3
````

4. This Ansible task installs project dependencies using npm within the "/app" directory.
```` bash
    - name: Install project dependencies with npm
      command: /usr/bin/npm install
      args:
        chdir: /app
      tags:
        - 4
````

5. This Ansible task runs npm build within the "/app" directory, based on the distribution being RedHat or Ubuntu.
```` bash
    - name: Run npm build
      block:
          command: /usr/bin/npm run build
          args:
            chdir: /app
      when: ansible_distribution == "RedHat" or ansible_distribution == "Ubuntu"
      tags:
        - 5
````


