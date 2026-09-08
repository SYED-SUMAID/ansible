# 45. Copy File to VMs — Ad Hoc

## Command

ansible all -m copy -a "src=test.txt dest=/tmp/test.txt"
![alt text](<Screenshot (709).png>)

## Explanation

- `ansible` — Runs an Ansible command.
- `all` — Targets all hosts defined in the Ansible inventory.
- `-m copy` — Uses the Ansible copy module to copy files from the control machine to target VMs.
- `-a` — Stands for arguments and provides arguments to the module.
- `src=test.txt` — Specifies the source file on the Ansible control machine.
- `dest=/tmp/test.txt` — Specifies the destination path on the target VMs.
- `src` — Means source.
- `dest` — Means destination.

The command copies `test.txt` from the Ansible control machine to `/tmp/test.txt` on all target VMs.

## Verify the File

ansible all -m command -a "ls -l /tmp/test.txt"
![alt text](<Screenshot (710).png>)

- `-m command` — Uses the Ansible command module.
- `ls -l /tmp/test.txt` — Checks whether the copied file exists on the target VMs.

## Copying to a Protected Directory

If the destination requires root privileges, use `-b`:

ansible all -m copy -a "src=test.txt dest=/etc/test.txt" -b

Here, `-b` enables privilege escalation so Ansible can write to the protected `/etc` directory.