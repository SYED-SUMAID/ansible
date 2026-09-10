# Ansible Dynamic Portfolio

> Automated deployment of a dynamic PHP portfolio using Ansible, Apache, PHP, and PostgreSQL.

## Overview

This project demonstrates automated deployment of a dynamic PHP web application using Ansible.

Ansible installs and configures the required services, creates the PostgreSQL database, deploys the PHP application, initializes the database, and restarts Apache.

## Technologies

- Ansible
- Linux / Ubuntu
- Apache HTTP Server
- PHP
- PostgreSQL
- SQL
- Git / GitHub

## Project Structure

    ansible-dynamic-portfolio/
    ├── README.md
    ├── setup.yml
    ├── inventory.ini
    └── files/
        ├── index.php
        └── init.sql

## Ansible Playbook

The main automation is handled by `setup.yml`.

The playbook:

1. Updates the APT package cache
2. Installs Apache, PHP, PostgreSQL, and required dependencies
3. Starts and enables Apache and PostgreSQL
4. Creates the PostgreSQL database and user
5. Grants database privileges
6. Deploys `index.php` to Apache
7. Copies and executes `init.sql`
8. Restarts Apache

The complete playbook is available in [`setup.yml`](setup.yml).

### Main Playbook Structure

    - name: Setup Dynamic PHP Portfolio
      hosts: localhost
      become: true

      tasks:
        - name: Install Apache PHP PostgreSQL
          ansible.builtin.apt:
            name:
              - apache2
              - php
              - libapache2-mod-php
              - php-pgsql
              - postgresql
              - postgresql-contrib
              - python3-psycopg2
            state: present

        - name: Create PostgreSQL database
          become_user: postgres
          community.postgresql.postgresql_db:
            name: vn7
            state: present

        - name: Create PostgreSQL user
          become_user: postgres
          community.postgresql.postgresql_user:
            name: code
            password: "12345"
            state: present

## Inventory

`inventory.ini` defines the Ansible hosts used by the project.

    [webservers]
    192.168.1.28
    192.168.1.30

    [local]
    localhost ansible_connection=local

## PHP Application

`files/index.php` is the dynamic frontend.

It:

- Connects to PostgreSQL
- Queries the `sm_users` table
- Retrieves profile information
- Generates profile cards dynamically
- Displays the data through the web interface

## Database Initialization

`files/init.sql` handles database initialization.

It:

- Creates the `sm_users` table
- Grants permissions to the application user
- Clears existing records
- Inserts portfolio data

## Database

| Setting | Value |
|---|---|
| Database | `vn7` |
| User | `code` |
| Table | `sm_users` |

## Ansible Collection

    ansible-galaxy collection install community.postgresql

## Deployment

Run the playbook from the project directory:

    ansible-playbook setup.yml

## Deployment Flow

    Ansible
       ↓
    Install Required Packages
       ↓
    Configure Apache + PostgreSQL
       ↓
    Create Database + User
       ↓
    Deploy index.php
       ↓
    Execute init.sql
       ↓
    Restart Apache
       ↓
    Dynamic Portfolio

## Terminal Proof

### Playbook Execution

    ansible-playbook setup.yml

### Ansible Connectivity

    ansible localhost -i inventory.ini -m ansible.builtin.ping

### Apache Status

    sudo systemctl status apache2

### PostgreSQL Status

    sudo systemctl status postgresql

### Database Tables

    sudo -u postgres psql -d vn7 -c "\dt"

### Database Records

    sudo -u postgres psql -d vn7 -c "SELECT * FROM sm_users;"

### Deployed Application

    ls -l /var/www/html/index.php

### Website Test

    curl http://localhost

## Proof 

- Successful Ansible playbook execution
![alt text](<Screenshot (730)(1).png>)
- Apache running
![alt text](<Screenshot 2026-09-10 230123.png>)
- PostgreSQL running
![alt text](<Screenshot 2026-09-10 230024.png>)
- Database table and records
![alt text](<Screenshot 2026-09-10 230417.png>)
- Final portfolio in the browser
![alt text](<Screenshot (731)(1).png>)
- GitHub repository structure
![alt text](<Screenshot (733)(1).png>)

## Key Concepts

- Ansible Playbooks
- Inventory Management
- FQCN Modules
- Privilege Escalation
- Apache Deployment
- PHP and PostgreSQL Integration
- Database Automation
- Linux Service Management
- Git and GitHub

## Result

A fully automated deployment of a dynamic PHP portfolio where Apache serves the application, PHP handles the frontend logic, and PostgreSQL provides the dynamic data.