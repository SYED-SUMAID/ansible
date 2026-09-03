# Ansible Basics

## 1. Inventory

Define the managed servers in an inventory file.

    [webservers]
    192.168.1.28 ansible_user=sum
    192.168.1.30 ansible_user=sum

## 2. Ansible Command Structure

    ansible <host> -m <module> -a "<arguments>"

- `-m` → Module
- `-a` → Module arguments
- `--become` → Run with elevated/root privileges

## 3. Command Module

Used to run simple Linux commands.

    ansible 192.168.1.30 -m command -a "ls"
    ansible 192.168.1.30 -m command -a "whoami"
    ansible 192.168.1.30 -m command -a "cat /tmp/test1.txt"

## 4. Shell Module

Used when shell features such as `>`, `>>`, `|`, and `&&` are required.

    ansible 192.168.1.30 -m shell -a "echo 'Long live Verventech' > /tmp/test.txt"

Rule:
- `command` → Simple commands
- `shell` → Commands requiring shell features

## 5. File Module

Used to create or remove files and directories.

### Create Directory

    ansible 192.168.1.30 -m file -a "path=/tmp/testdir state=directory"

### Create File

    ansible 192.168.1.30 -m file -a "path=/tmp/test1.txt state=touch"

### Delete File

    ansible 192.168.1.30 -m file -a "path=/tmp/test1.txt state=absent"

### Delete Directory

    ansible 192.168.1.30 -m file -a "path=/tmp/testdir state=absent"

## 6. Copy Module

Copies files from the control node to the managed node.

    ansible 192.168.1.30 -m copy -a "src=/tmp/test.txt dest=/tmp/test.txt"

Create a file with content:

    ansible 192.168.1.30 -m copy -a "content='Long live Verventech' dest=/tmp/test.txt"

- `src` → Control node
- `dest` → Managed node

## 7. APT Module

Used to install or remove packages on Debian/Ubuntu systems.

### Install Package

    ansible 192.168.1.30 -m apt -a "name=apache2 state=present" --become

### Install Multiple Packages

    ansible 192.168.1.30 -m apt -a "name=git,curl,python3 state=present" --become

### Remove Package

    ansible 192.168.1.30 -m apt -a "name=apache2 state=absent" --become

## 8. Service Module

Used to manage services.

### Start

    ansible 192.168.1.30 -m service -a "name=apache2 state=started" --become

### Stop

    ansible 192.168.1.30 -m service -a "name=apache2 state=stopped" --become

### Restart

    ansible 192.168.1.30 -m service -a "name=apache2 state=restarted" --become

### Enable at Boot

    ansible 192.168.1.30 -m service -a "name=apache2 enabled=yes" --become

## 9. Targeting Hosts

### All Hosts

    ansible all -m ping

### Specific Host

    ansible 192.168.1.30 -m ping

### Host Group

    ansible webservers -m ping

## 10. Important Modules

| Module | Purpose |
|---|---|
| `ping` | Test Ansible connectivity |
| `command` | Run simple commands |
| `shell` | Run shell commands |
| `file` | Manage files and directories |
| `copy` | Copy files to managed nodes |
| `apt` | Install/remove packages |
| `service` | Manage services |

## Basic Pattern

    ansible <host> -m <module> -a "<arguments>"

Ansible modules describe the desired state of the system, making tasks idempotent when the module supports it.

![alt text](<Screenshot (688).png>)
![alt text](<Screenshot (689).png>)
![alt text](<Screenshot (690).png>)
![alt text](<Screenshot (691).png>)
![alt text](<Screenshot (693).png>)
![alt text](<Screenshot (695).png>)
![alt text](<Screenshot (696).png>)
![alt text](<Screenshot (699).png>)
![alt text](<Screenshot (700).png>)