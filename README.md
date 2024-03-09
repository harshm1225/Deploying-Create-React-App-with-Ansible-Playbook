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

