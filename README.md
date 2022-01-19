# Ansible role MariaDB

[![CI Molecule](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml)

Molecule testing:

| Platforms |    Debian     |    Ubuntu     |    CentOS     |  Rocky Linux |
| --------- | ------------- | ------------- | ------------- | ------------ |
|  Version  |   10, 11      | 18.04, 20.04  |     7, 8      |      8       |
| optional repo|  mariadb.org    | mariadb.org    |  mariadb.org    |  mariadb.org   |

Optional tasks:

  - install MariaDB {version}  
  - copy your configuration ( ./my.cnf ) 
  - setup
    - users
    - roles
    - database
    - dump
    - query


Installation:

1) Install from Galaxy
```
ansible-galaxy install darexsu.mariadb
```
2) Example playbook: installation

```yaml
---
- hosts: all
  become: true
  
  roles:
    - role: darexsu.mariadb    
      vars:    
        # install
        mariadb_install: true    
```
2.2) Example playbook: update configuration

```yaml
---
- hosts: all
  become: true
  
  roles:
    - role: darexsu.mariadb    
      vars:    
        # settings
        mariadb_settings: true                                                       
        # settings -> copy config file
        mariadb_settings__copy_my_config_file_from: "./my"                            
        mariadb_settings__paste_my_config_file_to: "/etc/mysql/mariadb.conf.d/my.cnf"
```
2.3) Example playbook: create database 
```yaml
---
- hosts: all
  become: true
  
  roles:
    - role: darexsu.mariadb    
      vars:    
        # actions
        mariadb_actions: true
        mariadb_actions__login_user: "root"
        mariadb_actions__login_password: ""     
        # actions -> database -> manage
        mariadb_actions__database_manage: true
        mariadb_actions__database_manage_state: "present"
        mariadb_actions__database_manage_name: [test_database_name]
        mariadb_actions__database_manage_encoding: "utf8" 
```