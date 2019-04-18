# Description
This playbook provisions an mkcloud within an existing OpenStack installation.

An example usage is...
```
$ ansible-playbook mkcloud_create.yml --inventory hosts/localhost --extra-vars "stack_name=myusername_stack1 heat_template_filename=heat_template.yml ssh_keypair_name=myusername_kp ssh_keypair_filename=myusername_kp.pem mkcloud_vars_filename=develcloud.sh"
```

# Usage

## Parameters

* stack_name - A unique stack name
* heat_template_filename - The filename of the heat template for the new stack
* ssh_keypair_name - The name of your keypair (from `openstack keypair list`)
* ssh_keypair_filename - The filename of your private key (for SSH into the stack)
* mkcloud_vars_filename - The name of the file containing your mkcloud variable exports (**see [`example`](develcloud.sh.example): remove any explicit mkcloud call, add heat_\* exports, etc.**)

### Optional Parameters
* playbook_steps_start - The playbook runs through several steps in the process of provisioning an mkcloud. Use this parameter to skip ahead to the step that last failed. Valid values are: create_stack, git_mkcloud, copy_mkcloud_var_script, prepareinstcrowbar, bootstrapcrowbar, instcrowbar, crowbar_register, post_allocate, proposal

## Steps

Step #1: Create the venv (follow the section on the [`README`](README.md))

Step #2: Set the playbook parameter values
```
stack_name=myusername_stack1
heat_template_filename=heat_template.yml
ssh_keypair_name=myusername_kp
ssh_keypair_filename=myusername_kp.pem
mkcloud_vars_filename=develcloud.sh
playbook_steps_start=create_stack
```

Step #3: Set the environment variables
```
# source your personal OSRC file (downloaded from https://$horizon/project/api_access/openrc/)
$ . openstack.osrc

# source your mkcloud exports file to export the heat_* template values
$ . $mkcloud_vars_filename
```

Step #4: Run the playbook
```
$ ansible-playbook -vv mkcloud_create.yml --inventory hosts/localhost --extra-vars "stack_name=$stack_name heat_template_filename=$heat_template_filename ssh_keypair_name=$ssh_keypair_name ssh_keypair_filename=$ssh_keypair_filename mkcloud_vars_filename=$mkcloud_vars_filename playbook_steps_start=$playbook_steps_start" 2>&1 | tee mkcloud_create.log
```

# Improvements

* The ability to define separate controller and compute planes within the heat template. Requires:
  - An additional OS::Heat::ResourceGroup in the heat template
  - Changes to qa_crowbarsetup.sh:cluster_node_assignment()

# Common Issues

**Issue**: Nodes get stuck in the readying state \
**Resolution**: Run the following on the readying node to resolve
```
$ for s in readying ready; do
    crowbarctl node transition `hostname`.virtual.cloud.suse.de $s;
  done
```
\
**Issue**: Nodes get stuck in the reboot state \
**Resolution**: Run the following on admin to determine which nodes and manually
reboot them
```
$ for n in `knife node list`; do
    knife node show -a name -a state -a ipaddress -a default.crowbar.network.admin.address $n; echo;
  done
```
\
**Issue**: Several proposal commits time out (e.g. pacemaker, database, keystone,
neutron, etc) \
**Resolution**: Manually apply the failed proposal and rerun the playbook with
"playbook_steps_start=proposal"
\
\
**Issue**: Pacemaker proposal fails with "Cannot find device /dev/disk/by-id/scsi-!" \
**Resolution**: Run the [gist](https://gist.github.com/scottwulf/fb076c5dc1db124729d25f904c23e0b7) on the admin node and rerun the playbook starting with the proposal step
