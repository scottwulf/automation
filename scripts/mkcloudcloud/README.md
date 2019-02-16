# Description
This directory contains various tools which SUSE uses to provision mkcloud within
an existing OpenStack installation.


To create an mkcloud, run the [`mkcloud_create`](mkcloud_create.md) playbook.

To delete an mkcloud, run the [`mkcloud_delete`](mkcloud_delete.md) playbook.


# Usage
These tools depend on a few modules to be installed prior to use.
It is recommended to create a virtual Python environment so as not to interfere
with or be interfered by other installed software.

### Creation of venv
```
# create and activate a virtual env
$ virtualenv venv
$ . venv/bin/activate

# install/upgrade the necessary modules
$ pip install --upgrade ansible openstackclient openstacksdk pip
```

# Development

### Software Versions

The following versions of softwares were used during development.

| Name  | Version |
| ------------- | ------------- |
|develcloud  | 7, 8, 9  |
| Python  | 2.7.13  |
| pip  | 19.0.1  |
| ansible  | 2.7.6 |
| openstackclient  | 0.1.0  |
| openstacksdk  | 0.23.0  |
