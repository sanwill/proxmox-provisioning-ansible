# VM Provisioning using Ansible in Proxmox

There are view ways to provision VM in Proxmox, users can utilize the GUI, command line or Terraform.  
I used to use Ansible playbook to provision but I prefer Terraform.  
With terraform I don't have to preconfigured template VM nor create separated tasks to update the VM IP because I can simply configure init-config setting of the VM.

## Prerequisite
1. Ubuntu template VM with these following pre-requisites:
   - pre-configured static IP reachable from Ansible control node
   - openssh-server is installed and enabled
   - SSH user is created and added to suoers list
   - Python 3.5 or higher is installed at VM

2. Update the inventory.yaml file
   
   I use inventory file in yaml format because it is easier to pass it to the loop in the ansible playbook.
   See the example in the inventory.yaml

3. SSH keypair is generated at Ansible control node
   This key will be used to create passwordless SSH connection to VM. 

## Playbook Flow

1. Create VM from template using "Clone VM & Start VM" task
   
   The VM is created using Proxmox CLI via shell module of Ansible instead of using proxmox module. I found the CLI give more option to for the VM creation.

 3. Copy the public SSH key to VM using "ssh-copy" task
    
      In lab scenario user can opt to copy the SSH public key to template VM before convert the VM to Proxmox VM template. But here I assume that my plabook control node.

 4. Update VM IP using "Update OAM IP" task
    
 5. Next Ansible will reach to the VM using updated IP and execute additional configuration needed for the VM.

      In the example, the playbook will set the VM hostname and install qemu-guest-agent.

## Execution

```
ansible-playbook -i inventory.yaml proxmoxvm_setup.yaml --extra-vars "ansible_become_password=<user password>" --ssh-common-args='-o StrictHostKeyChecking=no'
```




