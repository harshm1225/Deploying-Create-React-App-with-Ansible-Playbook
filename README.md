# Deploying Create React App with Ansible Playbooks

## Overview
This documentation provides a step-by-step guide on how to deploy a React application using Ansible playbooks to both a Red Hat and Ubuntu server. The process involves setting up Ansible playbooks to automate the deployment process for Create React App on different server environments.

## Prerequisites
- Basic understanding of Ansible
- Access to Red Hat and Ubuntu servers
- Create React App project configured for deployment
- Ansible installed on the control machine

## Understanding Tasks in Ansible Playbook

1. Install Node.js and Git on RedHat machine
```` bash
# Install Node.js and Git on RedHat machine
- name: Install Node.js and Git  # Descriptive task name
  dnf:                          # Module used for package management
    name:                         # List of packages to install
      - nodejs
      - npm
      - git
    state: present               # Desired state (installed)
    update_cache: yes            # Update package cache before install
  when: ansible_distribution=="RedHat"  # Conditional execution for RedHat
  tags:                          # Optional tags for grouping tasks
    - 1
````



