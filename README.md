# Ansible role MariaDB

[![CI Molecule](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/57634?color=blue&label=downloads)

|  Official repo   |  Third-Party repo  |
| ---------------- | ------------------ |
| Debian 11        |  mariadb.org       |
| Debian 10        |  mariadb.org       |
| Ubuntu 20.04     |  mariadb.org       |
| Ubuntu 18.04     |  mariadb.org       |
| RockyLinux 8     |  mariadb.org       |
| OracleLinux 8    |  mariadb.org       |

### 1) Install role from Galaxy
```
ansible-galaxy install darexsu.mariadb --force
```

### 2) Example playbooks: 

 NOTE: You have to enable "hash_behaviour=merge" in ansible.cfg to use example playbooks

- [full playbook](#example-playbook-full-playbook)
  - install
    - [MariaDB from official repo](#example-playbook-install-mariadb-from-official-repo)
    - [MariaDB {version} from third-party repo](#example-playbook-install-mariadb-from-third-party-repo)
  - config
    - [copy config](#example-playbook-copy-config)

Replace dictionary and Merge dictionary (with "hash_behaviour=replace" in ansible.cfg):
```
[host_vars]           [host_vars]
---                   ---
  vars:                 vars:
    dict:                 merge:  <-- # Enable Merge
      a: "value"            dict: 
      b: "value"              a: "value" 
                              b: "value"
```
Role recursive merge:
```
[host_vars]     [current role]    [include_role]
  
  dict:          dict:              dict:
    a: "1" -->     a: "1"    -->      a: "1"
                   b: "2"    -->      b: "2"
                                      c: "3"
    
```

##### Example playbook: full playbook
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      mariadb_install:      
        enabled: true
        packages: [mariadb-server]
        dependencies:
          debian: [gnupg2, python3, python3-pymysql]
          redhat: [python3, python3-PyMySQL]
      mariadb_config:
        enabled: true   
        file:
          debian: "50-server.cnf"
          redhat: "server.cnf"   
        src: "~/path_to/my_template.j2"
        backup: true
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```

##### Example playbook: install MariaDB from official repo
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      mariadb_install:      
        enabled: true
        packages: [mariadb-server]
        dependencies:
          debian: [gnupg2, python3, python3-pymysql]
          redhat: [python3, python3-PyMySQL]
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```
##### Example playbook: install MariaDB from third-party repo
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      mariadb_install:      
        enabled: true
        packages: [mariadb-server]
        dependencies:
          debian: [gnupg2, python3, python3-pymysql]
          redhat: [python3, python3-PyMySQL]
      mariadb_repo:
        enabled: true
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```

##### Example playbook: copy config
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      mariadb_config:
        enabled: true   
        file:
          debian: "50-server.cnf"
          redhat: "server.cnf"   
        src: "~/path_to/my_template.j2"
        backup: true
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```