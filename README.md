# Ansible role MariaDB

[![CI Molecule](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/57634?color=blue&label=downloads)

|  Testing         |  Official repo     |  Third-party repo |
| :--------------: | :----------------: | :-------------:   |
| Debian 11        |  mariadb 10.5      |    mariadb.org    |
| Debian 10        |  mariadb 10.3      |    mariadb.org    |
| Ubuntu 20.04     |  mariadb 10.3      |    mariadb.org    |
| Ubuntu 18.04     |  mariadb 10.1      |    mariadb.org    |
| Oracle Linux 8   |  mariadb 10.3-10.5 |    mariadb.org    |
| Rocky Linux 8    |  mariadb 10.3-10.5 |    mariadb.org    |

### 1) Install role from Galaxy
```
ansible-galaxy install darexsu.mariadb --force
```

### 2) Example playbooks: 

- [full playbook](#full-playbook)
  - install
    - [install from official repo](#install-mariadb-from-official-repo)
    - [install {version} from third-party repo](#install-mariadb-from-third-party-repo)
  - config
    - [configure server.cnf](#configure-servercnf)

Role behaviour: Replace or Merge (with "hash_behaviour=replace" in ansible.cfg):
```
# Replace             # Merge
[host_vars]           [host_vars]
---                   ---
  vars:                 vars:
    dict:                 merge:
      a: "value"            dict: 
      b: "value"              a: "value" 
                              b: "value"

# Role recursive merge:
[host_vars]     [current role]    [include_role]
  
  dict:          dict:              dict:
    a: "1" -->     a: "1"    -->      a: "1"
                   b: "2"    -->      b: "2"
                                      c: "3"
    
```

##### full playbook
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true  
        src: "distribution"
      # MariaDB -> install
      mariadb_install:
        enabled: true
      # MariaDB -> config server.cnf 
      mariadb_config:
        enabled: true   
        file: "{{ mariadb_const[ansible_os_family]['config_file']}}"
        src: "mariadb__server.conf.j2"  
        backup: false
        vars:
          user: "mysql"
          pid-file: "{{ mariadb_const[ansible_os_family]['pid-file'] }}"
          socket: "{{ mariadb_const[ansible_os_family]['socket'] }}"
          basedir: "/usr"
          datadir: "/var/lib/mysql"    
          tmpdir: "/tmp"
          lc-messages-dir: "/usr/share/mysql"
          lc-messages: "en_US"
          bind-address: "127.0.0.1"
          expire_logs_days: "10"
          character-set-server: "utf8mb4"
          collation-server: "utf8mb4_general_ci"
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```

##### install MariaDB from official repo
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
      # MariaDB -> enable official repo   
        src: "distribution"
      # MariaDB -> install
      mariadb_install:
        enabled: true
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```
##### install MariaDB from third-party repo
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true  
      # MariaDB -> enable third_party repo 
        src: "third_party"
        version: "10.6"
        service:
          state: "started"
          enabled: true    
      # MariaDB -> enable third_party repo
      mariadb_install:
        enabled: true
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```

##### configure server.cnf
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
      # MariaDB -> config server.cnf 
      mariadb_config:
        enabled: true   
        file: "{{ mariadb_const[ansible_os_family]['config_file']}}"
        src: "mariadb__server.conf.j2"  
        backup: false
        vars:
          user: "mysql"
          pid-file: "{{ mariadb_const[ansible_os_family]['pid-file'] }}"
          socket: "{{ mariadb_const[ansible_os_family]['socket'] }}"
          basedir: "/usr"
          datadir: "/var/lib/mysql"    
          tmpdir: "/tmp"
          lc-messages-dir: "/usr/share/mysql"
          lc-messages: "en_US"
          bind-address: "127.0.0.1"
          expire_logs_days: "10"
          character-set-server: "utf8mb4"
          collation-server: "utf8mb4_general_ci"
  
  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```
