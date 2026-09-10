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

    ```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ansible Deployment</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(135deg, #0f172a, #1e1b4b, #312e81);
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 30px;
        }

        .container {
            width: 100%;
            max-width: 900px;
            text-align: center;
        }

        .badge {
            display: inline-block;
            padding: 8px 18px;
            border: 1px solid rgba(255,255,255,0.25);
            border-radius: 30px;
            background: rgba(255,255,255,0.08);
            color: #a5b4fc;
            font-size: 14px;
            margin-bottom: 25px;
        }

        h1 {
            font-size: 58px;
            margin-bottom: 18px;
            background: linear-gradient(90deg, #60a5fa, #a78bfa, #f472b6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .subtitle {
            font-size: 20px;
            color: #cbd5e1;
            margin-bottom: 40px;
        }

        .card-container {
            display: flex;
            gap: 20px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .card {
            width: 250px;
            padding: 28px 20px;
            border-radius: 18px;
            background: rgba(255,255,255,0.08);
            border: 1px solid rgba(255,255,255,0.15);
            backdrop-filter: blur(12px);
            transition: 0.3s ease;
        }

        .card:hover {
            transform: translateY(-8px);
            background: rgba(255,255,255,0.13);
            border-color: #818cf8;
        }

        .icon {
            font-size: 38px;
            margin-bottom: 15px;
        }

        .card h2 {
            font-size: 21px;
            margin-bottom: 10px;
        }

        .card p {
            color: #cbd5e1;
            line-height: 1.6;
            font-size: 14px;
        }

        .status {
            margin-top: 40px;
            display: inline-flex;
            align-items: center;
            gap: 10px;
            padding: 12px 22px;
            border-radius: 30px;
            background: rgba(34,197,94,0.12);
            border: 1px solid rgba(34,197,94,0.35);
            color: #86efac;
        }

        .dot {
            width: 10px;
            height: 10px;
            background: #22c55e;
            border-radius: 50%;
            box-shadow: 0 0 12px #22c55e;
        }

        footer {
            margin-top: 45px;
            color: #94a3b8;
            font-size: 13px;
        }
    </style>
</head>

<body>

    <div class="container">

        <div class="badge">
            AUTOMATED DEPLOYMENT
        </div>

        <h1>Welcome to My Website</h1>

        <p class="subtitle">
            Deployed automatically using Ansible and Apache HTTP Server
        </p>

        <div class="card-container">

            <div class="card">
                <div class="icon">⚙</div>
                <h2>Ansible</h2>
                <p>
                    Infrastructure automation and configuration management
                    made simple and repeatable.
                </p>
            </div>

            <div class="card">
                <div class="icon">▣</div>
                <h2>Apache</h2>
                <p>
                    A reliable web server delivering this website directly
                    to your browser.
                </p>
            </div>

            <div class="card">
                <div class="icon">✓</div>
                <h2>Deployment</h2>
                <p>
                    This website was copied and deployed automatically
                    through an Ansible playbook.
                </p>
            </div>

        </div>

        <div class="status">
            <span class="dot"></span>
            Deployment Successful
        </div>

        <footer>
            Built for learning • Linux • Ansible • Apache
        </footer>

    </div>

</body>
</html>
```

![alt text](<Screenshot (726)(1).png>)
![alt text](<Screenshot (727)(1).png>)
![alt text](<Screenshot (728)(1).png>)
![alt text](<Screenshot (729)(1).png>)

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
![alt text](<Screenshot (725)(1).png>)

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