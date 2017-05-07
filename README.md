# Vagrant Ansible WordPress setup

This is a simple set of code that is just from the [Digital Ocean 'WordPress using Ansible' tutorial][1].

Other ansible installs that I found when searching just had too much in them that I didn't want. This has the minimum to get WordPress working, then you can just fork it and customise it.

This is designed to run on localhost only, but you can modify it for specific servers.

This installs:

* Ubuntu 14.04 LTS
* Apache (libapache2-mod-php5)
* MySQL
* PHP 5.5.9 (php5, php5-mysql, php5-mcrypt, php5-gd, libssh2-php)
* Python-MySQL (for automated installation of MySQL DB)
* WordPress (latest)

## Prerequisites (i.e. tested with)

* VirtualBox v5.1.22 (this could be any of the VMs that Vagrant supports)
* Vagrant v1.8.5
* Ansible v2.3.0.0

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

## Notes

The hope is that you can understand all the code that is here - at least by following the tutorial. There really isn't much and it could be all in one `playbook.yml` file. All the code bloat comes from (which creates all the subdirectories under `roles`):

```
$ ansible-galaxy init server 
$ ansible-galaxy init php 
$ ansible-galaxy init mysql
$ ansible-galaxy init wordpress
```

I made a couple of modifications to the [Digital Ocean tutorial][1].

1. I include Vagrant in this as well, Vagrant and Ansible work very will together, but I believe that you can just use this code to run the playbook on its own
2. Effectively I rely on `vagrant up` to 'provision' the server which runs the ansible playbook.
3. If you modify the playbook after running vagrant up for the first time you have to run `vagrant provision` to force the re-run of the playbook
4. I am just using this for localhost development - so there is no `hosts` file and the `playbook.yml` file uses `hosts: all` which defaults to just localhost. The [Digital Ocean tutorial][1] has instructions for using other hosts.

This will get updated to Ubuntu 16 at some point, but this ansible playbook is fairly OS agnostic I think - so it *should* run on most Linux OSes.  

This is very little beyond a simple LAMP setup. You can delete the wordpress role and remove it from `playbook.yml` then you have a LAMP system that will create you a MySQL database.

  [1]: https://www.digitalocean.com/community/tutorials/how-to-automate-installing-wordpress-on-ubuntu-14-04-using-ansible

