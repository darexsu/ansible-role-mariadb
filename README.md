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
  
- [full playbook](#example-playbook-full-playbook)
  - install
    - [MariaDB from official repo](#example-playbook-install-mariadb-from-official-repo)
    - [MariaDB {version} from third-party repo](#example-playbook-install-mariadb-from-third-party-repo)
  - config
    - [copy config](#example-playbook-copy-config)

### Role hash_behaviour: Replace with defaults dictionaries
```yaml
    vars:
      dict:           # <-- Replace default dictionary
        a: my value
        b: my value
        c: my value
```
### Role hash_behaviour: Merge with default dictionaries
```yaml
    vars:
      mariadb_merge:    # <-- Merge with default dictionary
        dict:
          a: my value
          b: my value
          c: my value

```

##### Example playbook: full playbook
```yaml
---
- hosts: all
  become: true

  vars:
    mariadb_merge:
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
    mariadb_merge:
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
    mariadb_merge:
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
    mariadb_merge:
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