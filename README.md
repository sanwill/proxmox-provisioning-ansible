# VM Provisioning using Ansible in Proxmox

There are view to provision VM in Ansible, user can utilize the GUI, command line or Terraform.  
This is the Ansible playbook I used before I eventually use Terraform.

With terraform user don't have a preconfigured template VM nor a separated task to update the VM IP using netplan because user can configure init-config setting of the VM.

## Prerequisite
Ubuntu template VM with pre-configured static IP reachable from Proxmox host
openssh-server is installed and enabled
SSH user is created and added to sudoers list

## Some notes on the inventory file and playbook
I use inventory file in yaml format because it is easier to pass it to the loop in the ansible playbook.





## Execution
```
ansible-playbook -i inventory.yaml proxmoxvm_setup.yaml --extra-vars "ansible_become_password=<user password>" --ssh-common-args='-o StrictHostKeyChecking=no'
```




