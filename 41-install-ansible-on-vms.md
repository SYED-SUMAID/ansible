# Install Ansible on VMs

## 1. Update Package List

Update the system package list:

    sudo apt update

## 2. Install Required Package

Install `software-properties-common`:

    sudo apt install software-properties-common

## 3. Add Ansible Repository

Add the Ansible PPA and update the package list:

    sudo add-apt-repository --yes --update ppa:ansible/ansible

![alt text](<Screenshot (684)(1).png>)

## 4. Install Ansible

Install Ansible:

    sudo apt install ansible

![alt text](<Screenshot (685)(1).png>)

## 5. Verify Ansible Installation

Check the installed Ansible version:

    ansible --version

## 6. Create Ansible User

Create the dedicated `v-ansible` user:

    sudo adduser v-ansible

Follow the prompts to set the password and user details.

## 7. Add User to Sudo Group

Add `v-ansible` to the `sudo` group:

    sudo usermod -aG sudo v-ansible

## 8. Verify Sudo Group Membership

Check that `v-ansible` has been added to the `sudo` group:

    groups v-ansible

The output should include `sudo`.

## 9. Repeat on Other VMs

Repeat the same steps on the other VMs where Ansible and the `v-ansible` user are required.