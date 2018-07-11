# Vagrant Ansible WordPress setup

This is a set of code that is from the [Digital Ocean 'WordPress using Ansible' tutorial][1].

It has been upgraded to work on Ubuntu 16.04 rather than 14.04 as in the tutorial.

Other ansible installs that I found when searching just had too much in them that I didn't want. This has the minimum to get WordPress working, then you can just fork it and customise it.

This is designed to run on localhost only, but you can modify it for specific servers.

This installs:

* Ubuntu 16.04 LTS
* Apache (libapache2-mod-php)
* MySQL
* PHP 7 (php, php-mysql, php-mcrypt, php-gd, libssh2-php)
* Python-MySQL (for automated installation of MySQL DB)
* WordPress (latest)

## Prerequisites (i.e. tested with)

* Ansible v2.6.0
* VirtualBox v5.2.14 (this could be any of the VMs that Vagrant supports)
* Vagrant v2.0.2

## Installation

```bash
$ git clone https://github.com/BlankSlateCode/wordpress-ansible-vagrant.git
$ cd wordpress-ansible-vagrant
$ cp provisioning/roles/mysql/defaults/main-example.yml provisioning/roles/mysql/defaults/main.yml
```

Edit `provisioning/roles/mysql/defaults/main.yml` and set the values:

```yml
wp_mysql_db: my_database
wp_mysql_user: my_user
wp_mysql_password: my_password
```

```bash
$ vagrant up --provider virtualbox
```

Once the vagrant is running you can go to <http://localhost:8080> - which should show a WordPress installation page.

## No Vagrant

This project comes from a tutorial that is purely Ansible based with no requirement for Vagrant. So the playbook is still perfectly runnable on it's own.

So the installation is exactly as the Vagrant installation except for the final command:

```bash
$ cd provisioning
$ ansible-playbook playbook.yml -i my-inventory.txt
```

This will run the playbook on `localhost`. You can change `my-inventory.txt` to be what ever server you want to run the Ansible playbook against.

## Notes

The hope is that you can understand all the code that is here - at least by following the tutorial. There really isn't much and it could be all in one `playbook.yml` file. All the code bloat comes from the following commands (which create all the subdirectories under `roles`):

```
$ ansible-galaxy init server 
$ ansible-galaxy init php 
$ ansible-galaxy init mysql
$ ansible-galaxy init wordpress
```

I made a few modifications to the [Digital Ocean tutorial][1]:

1. I include Vagrant in this as well, Vagrant and Ansible work very will together, but I believe that you can just use this code to run the playbook on its own - see the section on 'No Vagrant' above.
2. Effectively I rely on `vagrant up` to 'provision' the server which runs the ansible playbook.
3. If you modify the playbook after running vagrant up for the first time you have to run `vagrant provision` to force the re-run of the playbook
4. I am just using this for localhost development - so there is no `hosts` file and the `playbook.yml` file uses `hosts: all` which defaults to just localhost. The [Digital Ocean tutorial][1] has instructions for using other hosts.
5. I have upgraded it to Ubuntu 16.04.

This ansible playbook is fairly OS agnostic I think - so it *should* run on most Linux OSes.

This is very little beyond a simple LAMP setup. You can delete the wordpress role and remove it from `playbook.yml` then you have a LAMP system that will create you a MySQL database.

  [1]: https://www.digitalocean.com/community/tutorials/how-to-automate-installing-wordpress-on-ubuntu-14-04-using-ansible

