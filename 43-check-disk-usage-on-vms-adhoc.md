# 43. Check Disk Usage on VMs — Ad Hoc

## Command

ansible all -m shell -a "df -h"
![alt text](<Screenshot (707).png>)

## Explanation

- `ansible` — Runs an Ansible command.
- `all` — Targets all hosts defined in the Ansible inventory.
- `-m shell` — Uses the Ansible shell module to execute a shell command on the target VMs.
- `-a` — Stands for arguments and provides the command to the module.
- `df` — Displays disk space usage of filesystems.
- `-h` — Means human-readable and displays sizes such as KB, MB, and GB.

The command checks the disk usage on all target VMs.

## Example Output

Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   18G   30G  38% /

The `Use%` column shows the percentage of disk space currently being used.