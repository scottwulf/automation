# Description
This playbook removes an mkcloud from within an existing OpenStack installation.

An example usage is...
```
$ ansible-playbook mkcloud_delete.yml --inventory hosts/localhost --extra-vars "stack_name=myusername_stack1"
```

# Usage

## Parameters

* stack_name - The stack name

## Steps

Step #1: Create the venv (follow the section on the [`README`](README.md))

Step #2: Set the playbook parameter values
```
stack_name=myusername_stack1
```

Step #3: Set the environment variables
```
# source your personal OSRC file (downloaded from https://$horizon/project/api_access/openrc/)
$ . openstack.osrc
```

Step #4: Run the playbook
```
$ ansible-playbook -vv mkcloud_delete.yml --inventory hosts/localhost --extra-vars "stack_name=$stack_name" 2>&1 | tee mkcloud_delete.log
```
