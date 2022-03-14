# Ansible role MariaDB

[![CI Molecule](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/57634?color=blue&label=downloads)

  - Role:
      - [platforms](#platforms)
      - [install](#install)
      - [behaviour](#behaviour)
  - Playbooks (short version):
      - [install and configure: MariaDB](#install-and-configure-mariadb-short-version)
          - [install: MariaDB from official repo](#install-mariadb-from-official-repo-short-version)
          - [install: MariaDB from third-party repo](#install-mariadb-from-third-party-repo-short-version)
          - [configure: server.conf](#configure-serverconf-short-version)
  - Playbooks (full version):
      - [install and configure: MariaDB](#install-and-configure-mariadb-full-version)
          - [install: MariaDB from official repo](#install-mariadb-from-official-repo-full-version)
          - [install: MariaDB from third-party repo](#install-mariadb-from-third-party-repo-full-version)
          - [configure: server.conf](#configure-serverconf-full-version)

### Platforms

|  Testing         |  Official repo     |  Third-party repo |
| :--------------: | :----------------: | :-------------:   |
| Debian 11        |  mariadb 10.5      |    mariadb.org    |
| Debian 10        |  mariadb 10.3      |    mariadb.org    |
| Ubuntu 20.04     |  mariadb 10.3      |    mariadb.org    |
| Ubuntu 18.04     |  mariadb 10.1      |    mariadb.org    |
| Oracle Linux 8   |  mariadb 10.3-10.5 |    mariadb.org    |
| Rocky Linux 8    |  mariadb 10.3-10.5 |    mariadb.org    |

### Install

```
ansible-galaxy install darexsu.mariadb --force
```

### Behaviour

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

##### Install and configure: MariaDB (short version)
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
      # MariaDB -> config
      mariadb_config:
        enabled: true
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

##### Install: MariaDB from official repo (short version)
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

  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```
##### Install: MariaDB from third-party repo (short version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
        src: "third_party"
        version: "8.0"
      # MariaDB -> install
      mariadb_install:
        enabled: true

  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```

##### Configure: server.conf (short version)
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
      # MariaDB -> config
      mariadb_config:
        enabled: true
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
##### Install and configure: MariaDB (full version)
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
        version: ""
        service:
          state: "started"
          enabled: true
      # MariaDB -> install
      mariadb_install:
        enabled: true
        packages:
          Debian: [mariadb-server]
          RedHat: [mariadb-server]
        dependencies:
          Debian: [gnupg2, python3, python3-pymysql]
          RedHat: [python3, python3-PyMySQL]
      # MariaDB -> config
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

##### Install: MariaDB from official repo (full version)
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
        version: ""
        service:
          state: "started"
          enabled: true
      # MariaDB -> install
      mariadb_install:
        enabled: true
        packages:
          Debian: [mariadb-server]
          RedHat: [mariadb-server]
        dependencies:
          Debian: [gnupg2, python3, python3-pymysql]
          RedHat: [python3, python3-PyMySQL]

  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```
##### Install: MariaDB from third-party repo (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
        src: "third_party"
        version: "8.0"
        service:
          state: "started"
          enabled: true
      # MariaDB -> install
      mariadb_install:
        enabled: true
        packages:
          Debian: [mariadb-server]
          RedHat: [mariadb-server]
        dependencies:
          Debian: [gnupg2, python3, python3-pymysql]
          RedHat: [python3, python3-PyMySQL]

  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```

##### Configure: server.conf (full version)
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
        version: ""
        service:
          state: "started"
          enabled: true
      # MariaDB -> config
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