# 44. Install Git on VMs — Ad Hoc

## Command

ansible all -m apt -a "name=git state=present" -b
![alt text](<Screenshot (708).png>)

## Explanation

- `ansible` — Runs an Ansible command.
- `all` — Targets all hosts defined in the Ansible inventory.
- `-m apt` — Uses the Ansible apt module to manage packages on Ubuntu/Debian systems.
- `-a` — Stands for arguments and provides arguments to the module.
- `name=git` — Specifies Git as the package to install.
- `state=present` — Ensures that Git is installed. If Git is already installed, Ansible leaves it installed.
- `-b` — Stands for become and enables privilege escalation, usually using sudo.

## Verify Git Installation

ansible all -m command -a "git --version"
![alt text](<Screenshot 2026-09-08 174316.png>)

- `-m command` — Uses the Ansible command module.
- `git --version` — Displays the installed Git version.

## Example Output

git version 2.43.0