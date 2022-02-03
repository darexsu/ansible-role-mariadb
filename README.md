# Ansible role MariaDB

[![CI Molecule](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/57634?color=blue&label=downloads)

Molecule testing:

| Platforms |    Debian     |    Ubuntu     |    Rocky Linux |  Oracle Linux |
| --------- | ------------- | ------------- | ------------- | ------------ |
|  Version  |   10, 11      | 18.04, 20.04  |      8      |      8       |
| Repository|  mariadb.org    | mariadb.org    |  mariadb.org    |  mariadb.org   |

### 1) Install role from Galaxy
```
ansible-galaxy install darexsu.mariadb --force
```

### 2) Example playbooks: 
  
  - install
    - [MariaDB {version}](#example-playbook-install-mariadb)
  - config
    - [copy config](#example-playbook-copy-config)
  - actions
    - [user](#example-playbook-user)
    - [role](#example-playbook-role)
    - [database](#example-playbook-database-manage)
    - [dump](#example-playbook-dump)
    - [database import sql](#example-playbook-database-import-sql)
    - [query](#example-playbook-query)

##### Example playbook: install MariaDB
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- install
      mariadb_install: true
      mariadb_install__version: "10.6"

```

##### Example playbook: copy config
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- config
      mariadb_config: true
      # ----- config  copy file
      mariadb_config__copy_file_from: "my.cnf"
      mariadb_config__copy_file_to: "/etc/mysql/mariadb.conf.d/my.cnf"

```
##### Example playbook: user
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- actions
      mariadb_actions: true
      mariadb_actions__login_user: "root"
      mariadb_actions__login_password: ""
      # ----- actions  user
      mariadb_actions__user: true
      mariadb_actions__user__state: "present"
      mariadb_actions__user__name: "test_name"
      mariadb_actions__user__password: "test_password"
      mariadb_actions__user__privileges: '*.*:ALL'
```
##### Example playbook: role
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- actions
      mariadb_actions: true
      mariadb_actions__login_user: "root"
      mariadb_actions__login_password: ""
      # ----- actions  role
      mariadb_actions__role: true
      mariadb_actions__role__state: "present"
      mariadb_actions__role__name: "test_name"
```
##### Example playbook: database manage
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- actions
      mariadb_actions: true
      mariadb_actions__login_user: "root"
      mariadb_actions__login_password: ""
      # ----- actions  database manage
      mariadb_actions__database_manage: true
      mariadb_actions__database_manage__state: "present"
      mariadb_actions__database_manage__name: []
      mariadb_actions__database_manage__encoding: "utf8"
      mariadb_actions__database_manage__collation: ""
```
##### Example playbook: dump
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- actions
      mariadb_actions: true
      mariadb_actions__login_user: "root"
      mariadb_actions__login_password: ""
      # ----- actions  database dump
      mariadb_actions__database_dump: true
      mariadb_actions__database_dump__name: []
      mariadb_actions__database_dump__folder: "/tmp/"
      mariadb_actions__database_dump__encoding: "utf8"
```
##### Example playbook: database import sql
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- actions
      mariadb_actions: true
      mariadb_actions__login_user: "root"
      mariadb_actions__login_password: ""
      # ----- actions  database import sql
      mariadb_actions__database_import_sql: true
      mariadb_actions__database_import_sql__namedb: []
      mariadb_actions__database_import_sql__path: ""
      mariadb_actions__database_import_sql__encoding: ""
```

##### Example playbook: query
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- actions
      mariadb_actions: true
      mariadb_actions__login_user: "root"
      mariadb_actions__login_password: ""
      # ----- actions  database query
      mariadb_actions__database_query: true
      mariadb_actions__database_query__namedb: ""
      mariadb_actions__database_query__sql: []
```