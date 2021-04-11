# Wordpress LAMP Playbook

Navigate to the `wordpress-lamp_ubuntu1804` folder and use the `tree` command to see the following structure:
```shell
admin@ansible-master:~/wordpress-ansible/wordpress-lamp_ubuntu1804$ tree
.
├── files
│   ├── apache.conf.j2
│   ├── client.my.cnf
│   ├── client.my.cnf.j2
│   └── wp-config.php.j2
├── inventory
├── playbook.yml
├── readme.md
└── vars
    └── default.yml
```
## Settings
Here is the following description of what each of these files is: 
- `files/apahce.conf.j2`: Template file for setting up the Apache VirtualHost.
- `files/wp-config.php.j2`: Template file for setting up WordPress’s configuration file.
- `files/client.my.cnf`: The initial client.my.cnf is provided without a password and used to obtain a connection from which roots password updates to the managed nodes we want to store for MySQL database connection.
- `files/client.my.cnf.j2`: Contains the same structure as the initial client.my.cnf file but as a jinja2 Template file for better portability.
- `inventory`: Keeps track of which nodes and hosts will be a part of the infrastructure on which playbooks and ad-hoc commands will run.
- `vars/default.yml`: Variable file for customizing playbook settings.
- `playbook.yml`: The playbook.yml file is where all tasks from this setup are defined. It starts by defining the group of servers that should target this setup (all). It uses become: true to describe that tasks should be executed with privilege escalation (sudo) by default. Then, it includes the `vars/default.yml` variable file to load configuration options.

The following list contains a brief explanation of each of these variables if they should edit or change for any reason.
- `php_modules`: An array containing PHP 7.4 extensions that support your WordPress setup. This list is extensive to support more features.
- `mysql_root_password`: The desired password for the **root** MySQL account.
- `mysql_db`: The name of the MySQL database intended for WordPress.
- `mysql_user`: The name of the MySQL user for WordPress.
- `mysql_password`: The password for the new MySQL user.
- `http_host`: Your domain name.
- `http_conf`: The name of the configuration file within Apache.
- `http_port`: HTTP port for the virtual host. By default, it is `80`.

## Running this Playbook
### 1. Navigate to the playbook
```shell
admin@ansible-master:~/wordpress-ansible$ cd wordpress-lamp_ubuntu1804/
```

### 2. Customize Options

```shell
nano vars/default.yml
```

```yml
---
#System Settings
php_modules: [ 'php7.4-curl', 'php7.4-cli', 'php7.4-dev', 'php7.4-gd', 'php7.4-mbstring', 'php7.4-mcrypt', 'php7.4-json', 'php7.4-tidy', 'php7.4-opcache', 'php
7.4-xml', 'php7.4-xmlrpc', 'php7.4-pdo', 'php7.4-soap', 'php7.4-intl', 'php7.4-zip' ]

#MySQL Settings
mysql_root_password: "Passw0rd"
mysql_db: "Wordpress_db"
mysql_user: "db_user"
mysql_password: "admin123!"

#HTTP Settings
http_host: "your_domain"
http_conf: "your_domain.conf"
http_port: "80"
```

### 3. Run the Playbook

Once you’ve verified that the variables are correct, you can run the Playbook to install WordPress on the managed node(s) with the `ansible-playbook` command below:

```command
ansible-playbook -i inventory -u admin playbook.yml
```
When the Playbook finishes all of its tasks, you should see a Play recap of all the events. **“OK”** means the task is done and configured correctly from the last run, or **“changed”** if Ansible finds the task alters the current state configured on the managed node(s).
```shell
PLAY RECAP ****************************************************************************************************************************************************
ansible-node1              : ok=28   changed=10   unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ansible-node2              : ok=28   changed=11   unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```
To see if WordPress is up and running, navigate to the managed node’s domain name or IP address: `http://IP_address`
