# VM Provisioning using Ansible in Proxmox

There are view to provision VM in Ansible, user can utilize the GUI, command line or Terraform.  
This is the Ansible playbook I used before I eventually use Terraform.

With terraform user don't have a preconfigured template VM nor a separated task to update the VM IP using netplan because user can configure init-config setting of the VM.

## Prerequisite
1. Ubuntu template VM with these following pre-requisites:
   - pre-configured static IP reachable from Ansible server
   - openssh-server is installed and enabled
   - SSH user is created and added to suoers list

2. Update the inventory.yaml file
I use inventory file in yaml format because it is easier to pass it to the loop in the ansible playbook.
See the example in the inventory.yaml

## Playbook flow

1. Create VM from template using "Clone VM & Start VM" task
The VM is created using Proxmox CLI via shell module of Ansible instead of using proxmox module. I found the CLI give more option to for the VM creation

 2. Copy the public SSH key to VM using "ssh-copy" task

 3. Update VM IP using "Update OAM IP" task

 4. Next Ansible will reach to the VM using updated IP and execute additional configuration needed for the VM. In the example, the playbook will set the VM hostname and install qemu-guest-agent.

## Execution
```
ansible-playbook -i inventory.yaml proxmoxvm_setup.yaml --extra-vars "ansible_become_password=<user password>" --ssh-common-args='-o StrictHostKeyChecking=no'
```




