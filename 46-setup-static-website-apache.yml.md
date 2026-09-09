# Static Website Deployment Using Apache and Ansible

## Overview

This project deploys a static HTML website on an Ubuntu VM using Ansible and Apache2.

## Project Structure

apache-website/
- setup-static-website-apache.yml
- website/
  - index.html

## Step 1: Create Project Directory

    mkdir apache-website
    cd apache-website
![alt text](<Screenshot (711).png>)

## Step 2: Create Website Directory

    mkdir website

## Step 3: Create the Website

    nano website/index.html

Add:

    <!DOCTYPE html>
    <html>
    <head>
        <title>My Website</title>
    </head>
    <body>
        <h1>Hello World</h1>
        <p>This website was deployed using Ansible.</p>
    </body>
    </html>
![alt text](<Screenshot (716).png>)

## Step 4: Create the Ansible Playbook

    nano setup-static-website-apache.yml

Playbook:

    ---
    - name: Setup static website using Apache
      hosts: webservers
      become: yes

      tasks:
        - name: Install Apache
          apt:
            name: apache2
            state: present
            update_cache: yes

        - name: Start and enable Apache
          service:
            name: apache2
            state: started
            enabled: yes

        - name: Copy website files
          copy:
            src: website/
            dest: /var/www/html/

        - name: Set website ownership
          file:
            path: /var/www/html
            owner: www-data
            group: www-data
            recurse: yes

![alt text](<Screenshot (717).png>)

## Step 5: Configure Inventory

Edit the default Ansible inventory:

    sudo nano /etc/ansible/hosts

Add:

    [localhost]
    localhost ansible_connection=local
![alt text](<Screenshot (718).png>)

## Step 6: Test Ansible

    ansible localhost -m ping
![alt text](<Screenshot (720).png>)

Expected result:

    localhost | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }

## Step 7: Run the Playbook

    ansible-playbook setup-static-website-apache.yml

## Step 8: Verify

Check the website files:

    ls -l /var/www/html/

Open the website in a browser:

    http://localhost
![alt text](<Screenshot (714)(1).png>)

## Important Concepts

### `src: website/`

The local directory containing the website files.

### `dest: /var/www/html/`

Apache's default web directory where the website files are copied.

### `become: yes`

Allows Ansible to perform tasks with administrator privileges.

### `recurse: yes`

Applies the ownership change to the directory and everything inside it.

## Result

The static website was successfully deployed on the Ubuntu VM using Ansible and Apache2.