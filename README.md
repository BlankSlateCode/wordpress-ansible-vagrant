# Vagrant Ansible WordPress setup

This is a simple set of code that is just from the [Digital Ocean 'WordPress using Ansible' tutorial][1].

Other ansible installs that I found when searching just had too much in them that I didn't want. This has the minimum to get WordPress working, then you can just fork it and customise it.

This installs:

* Ubuntu 14.04 LTS (You can switch this to Ubuntu 16)
* Apache (libapache2-mod-php5)
* MySQL
* PHP 5.5.9 (php5, php5-mysql, php5-mcrypt, php5-gd, libssh2-php)
* Python-MySQL (for automated installation of MySQL DB)
* WordPress (latest)

## Prerequisites (i.e. tested with)

* Vagrant v1.8.5
* Ansible v2.3.0.0
* VirtualBox v5.1.22 (assumed - this could be any of the VMs that Vagrant supports)

## Installation

In the steps below you'll copy the example config file to `provisioning/roles/mysql/defaults/main.yml` and edit it

```yml
wp_mysql_db: my_database
wp_mysql_user: my_user
wp_mysql_password: my_password
```

```bash
$ git clone https://github.com/BlankSlateCode/wordpress-ansible-vagrant.git
$ cd wordpress-ansible-vagrant
$ cp provisioning/roles/mysql/defaults/main-example.yml provisioning/roles/mysql/defaults/main.yml
$ vim provisioning/roles/mysql/defaults/main.yml
$ vagrant up --provider virtualbox
```

Once the vagrant is running you can go to <http://localhost:8080> - which should show a WordPress installation page.


  [1]: https://www.digitalocean.com/community/tutorials/how-to-automate-installing-wordpress-on-ubuntu-14-04-using-ansible
