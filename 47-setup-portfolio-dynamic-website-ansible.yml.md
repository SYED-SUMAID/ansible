# Dynamic Apache Portfolio Deployment with Ansible

## Overview

This project automates the deployment of a dynamic PHP portfolio using Ansible.

## Technologies

- Ansible
- Apache2
- PHP
- PostgreSQL
- Git/GitHub

## Project Structure

    Portfolio-Shell-Automation/
    ├── index.php
    ├── init.sql
    ├── portfolio.yml
    └── README.md

## What the Playbook Does

1. Installs Apache, PHP, PostgreSQL, and Git.
2. Starts and enables Apache and PostgreSQL.
3. Clones the portfolio from GitHub.
4. Creates the PostgreSQL user `code`.
5. Creates the database `vn7`.
6. Executes `init.sql`.
7. Creates the `sm_users` table and inserts data.
8. Deploys the portfolio to `/var/www/html`.
9. Sets Apache ownership.
10. Restarts Apache.

## Database

- Database: `vn7`
- User: `code`
- Table: `sm_users`

The PostgreSQL user and database are created by Ansible.

Therefore, `init.sql` should contain only the table creation, permissions, and data insertion.

## Ansible Playbook

    ---
    - name: Setup Dynamic Apache Portfolio
      hosts: localhost
      connection: local
      become: yes

      vars:
        repo_url: "https://github.com/SYED-SUMAID/Portfolio-Shell-Automation.git"
        repo_dir: "/opt/Portfolio-Shell-Automation"
        web_dir: "/var/www/html"

      tasks:

        - name: Install required packages
          apt:
            name:
              - apache2
              - php
              - libapache2-mod-php
              - php-pgsql
              - postgresql
              - git
            state: present
            update_cache: yes

        - name: Start Apache
          service:
            name: apache2
            state: started
            enabled: yes

        - name: Start PostgreSQL
          service:
            name: postgresql
            state: started
            enabled: yes

        - name: Clone portfolio from GitHub
          git:
            repo: "{{ repo_url }}"
            dest: "{{ repo_dir }}"
            version: main
            force: yes

        - name: Create PostgreSQL user
          shell: |
            sudo -u postgres psql -tc "SELECT 1 FROM pg_roles WHERE rolname='code'" | grep -q 1 ||
            sudo -u postgres psql -c "CREATE USER code WITH PASSWORD '12345';"

        - name: Create PostgreSQL database
          shell: |
            sudo -u postgres psql -tc "SELECT 1 FROM pg_database WHERE datname='vn7'" | grep -q 1 ||
            sudo -u postgres psql -c "CREATE DATABASE vn7 OWNER code;"

        - name: Copy init.sql
          copy:
            src: "{{ repo_dir }}/init.sql"
            dest: "/tmp/init.sql"
            remote_src: yes

        - name: Setup database
          shell: |
            sudo -u postgres psql -d vn7 -f /tmp/init.sql

        - name: Deploy portfolio
          shell: |
            cp -r {{ repo_dir }}/* {{ web_dir }}/

        - name: Set Apache ownership
          file:
            path: "{{ web_dir }}"
            owner: www-data
            group: www-data
            recurse: yes

        - name: Restart Apache
          service:
            name: apache2
            state: restarted
![alt text](<Screenshot (721).png>)
![alt text](<Screenshot (722).png>)

## Run the Playbook

    ansible-playbook setup-portfolio-dynamic-website-ansible.yml
![alt text](<Screenshot (723).png>)

## Verify

Check Apache:

    sudo systemctl status apache2

Check PostgreSQL:

    sudo systemctl status postgresql

Open the portfolio:

    http://localhost
![alt text](<Screenshot (724)(1).png>)
## Deployment Flow

    GitHub
       ↓
    Ansible
       ↓
    Apache + PHP
       ↓
    PostgreSQL
       ↓
    Dynamic Portfolio