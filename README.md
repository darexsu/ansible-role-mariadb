# Ansible role MariaDB

[![CI Molecule](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/57634?color=blue&label=downloads)

|  Official repo   | Mariadb version    |  Third-Party repo |  Mariadb version | 
| ---------------- | ------------------ | ----------------- | ---------------- |
| Debian 11        |   10.5             | mariadb.org       |     Latest       | 
| Debian 10        |   10.3             | mariadb.org       |     Latest       |   
| Ubuntu 20.04     |   10.3             | mariadb.org       |     Latest       | 
| Ubuntu 18.04     |   10.1             | mariadb.org       |     Latest       |   
| RockyLinux 8     |   10.3             | mariadb.org       |     Latest       | 
| OracleLinux 8    |   10.3             | mariadb.org       |     Latest       | 

### 1) Install role from Galaxy
```
ansible-galaxy install darexsu.mariadb --force
```

### 2) Example playbooks: 
  
  - install
    - [MariaDB from official repo](#example-playbook-install-mariadb-from-official-repo)
    - [MariaDB {version} from third-party repo](#example-playbook-install-mariadb-from-third-party-repo)
  - config
    - [copy config](#example-playbook-copy-config)
  - actions
    - [user](#example-playbook-user)
    - [role](#example-playbook-role)
    - [database](#example-playbook-database-manage)
    - [dump](#example-playbook-dump)
    - [database import sql](#example-playbook-database-import-sql)
    - [query](#example-playbook-query)

##### Example playbook: install MariaDB from official repo
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- install
      mariadb_install: true

```
##### Example playbook: install MariaDB from third-party repo
```yaml
---
- hosts: all
  become: yes

  roles:
    - role: darexsu.mariadb
      # ----- install
      mariadb_install: true
      # ----- install  repo
      mariadb_install__repo__mariadb: true
      mariadb_install__repo__mariadb__version: "10.6"  

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