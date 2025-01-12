# Ansible Role: virt_install_import

An ansible role to import virtual machine with the ```virt-install import``` command

## Requirements

This role depends on the ```virt-install``` command.
The role will install the required packages on most GNU/Linux distributions.

### Supported GNU/Linux Distributions

* Archlinux
* AlmaLinux
* Debian
* Centos
* Fedora
* RedHat
* Rocky
* Suse
* Ubuntu

## Installation

### Ansible galaxy

The role is available on [Ansible Galaxy](https://galaxy.ansible.com/ui/standalone/roles/stafwag/virt_install_import/).

To install the role from Ansible Galaxy execute the command below.

```bash
$ ansible-galaxy role install stafwag.virt_install_import
```

### Source Code

If you want to use the source code directly.

Clone the role source code.

```bash
$ git clone https://github.com/stafwag/ansible-role-virt_install_import stafwag.virt_install_import
```

and put it into the [role search path](https://docs.ansible.com/ansible/2.4/playbooks_reuse_roles.html#role-search-path)

## Role tasks, tags variables and templates

### Tasks

* **install**

    All installation-related tasks are defined in the ```install``` playbook. This allows you to install the
    required packages and start/enable the required service with ```tasks_from``` in the ```include_role```,
    ```import_role```, … ansible modules.

    See example below.

### Tags

* **install**

  Install the required packages.

### Playbook related variables

* **virt_install_import**: "namespace"
  * **name**: required. The name of the virtual machine
  * **autostart**: optional default: true. Enable autostart.
  * **wait**: optional default: 0. The ```--wait``` option for the ```virt-install```
    command. A negative value will wait still the virtmachine is shutdown.
    0 will just start the virtual machine and disconnect. 
  * **memory**: optional, default 1024 The virtual machine memory
  * **vcpu**: optional, default 1. The number of virtual cpu cores.
  * **virt_type**: optional, default: kvm.  The hypervisor type.
  * **graphics**: optional, default: none. The graphical display configuration.
  * **os_type**: optional, default: not defined. The operating system type.
  * **os_variant**: optional, default: not defined. The operating system variant.
  * **network**: optional, default: not defined. The network string
  * **disks**: optional, default: not defined. The disk strings.

## Dependencies

None

## Example Playbooks

### Install the virt-install packages with include_role

```
---
- name: Install libvirt & co
  gather_facts: true 
  hosts: all
  become: true
  tasks:
    - name: Install the requirements
      include_role:
        name: "{{ item }}"
        tasks_from:
          install
      with_items:
        - stafwag.libvirt 
        - stafwag.qemu_img
        - stafwag.cloud_localds
        - stafwag.virt_install_import
```

### Import a virtual machine
 
```
---
- name: Import a virtual machine
  gather_facts: no 
  become: true
  hosts: localhost
  roles:
    - role: stafwag.virt_install_import
      vars:
        virt_install_import:
          wait: -1
          name: tstdebian2
          os_type: Linux
          os_variant: debian10
          network: network:default
          graphics: spice
          disks:
            - /var/lib/libvirt/images/tstdebian2.qcow2,device=disk
            - /var/lib/libvirt/images/tstdebian2_cloudinit.iso,device=cdrom
```


## License

MIT/BSD

## Author Information

Created by Staf Wagemakers, email: staf@wagemakers.be, website: https://www.wagemakers.be, my company https://mask27.dev
