# SSH and Admin user setup with Setup playbook

The `setup_ubuntu1804` folder in the Github repository runs an independent playbook that will execute an initial server setup for the managed nodes. The options stores in the `vars/default.yml` variable file. We define the following settings below:

## Settings

- `create_user`: The name of the remote sudo user to create. In our case, it will be admin.
- `copy_local_key`: Path to a local SSH public key that will be copied as authorized key for the new user. By default, it copies the key from the current system user running Ansible.
- `sys_packages`: An array with a list fundamental packages to be installed. 


## Running this Playbook

Quick Steps:

### 1. Navigate to the playbook
```shell
admin@ansible-master:~/wordpress-ansible$ cd setup_ubuntu1804/
```

### 2. Customize Options

```shell
nano vars/default.yml
```

```yml
#vars/default.yml
---
create_user: admin
copy_local_key: "{{ lookup('file', lookup('env','HOME') + '/.ssh/id_rsa.pub') }}"
sys_packages: [ 'curl', 'vim', 'git', 'tree', 'ufw']
```

### 3. Run the Playbook

```command
admin@ansible-master:~/.../setup_ubuntu1804$ ansible-playbook -i inventory -u admin playbook.yml
```

Once the Playbook completes as successful, you can test the SSH connection with the following Ansible commands for our inventory with `ansible all -i inventory -m ping` and `ansible-inventory -i inventory --list`.

```shell
admin@ansible-master:~/.../setup_ubuntu1804$ ansible all -i inventory -m ping
ansible-node2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
ansible-node1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

admin@ansible-master:~/.../wordpress-lamp_ubuntu1804$ ansible-inventory -i inventory --list
{
    "_meta": {
        "hostvars": {
            "ansible-node1": {
                "ansible_host": "10.23.45.20",
                "ansible_python_interpreter": "/usr/bin/python3"
            },
            "ansible-node2": {
                "ansible_host": "10.23.45.30",
                "ansible_python_interpreter": "/usr/bin/python3"
            }
        }
    },
    "all": {
        "children": [
            "servers",
            "ungrouped"
        ]
    },
    "servers": {
        "hosts": [
            "ansible-node1",
            "ansible-node2"
        ]
    }
}
```
