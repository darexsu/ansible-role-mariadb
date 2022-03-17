# Ansible role MariaDB

[![CI Molecule](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-mariadb/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/57634?color=blue&label=downloads)

  - Role:
      - [platforms](#platforms)
      - [install](#install)
      - [merge behaviour](#merge-behaviour)
  - Playbooks (merge version):
      - [install and configure: MariaDB](#install-and-configure-mariadb-merge-version)
          - [install: MariaDB, repository: distribution](#install-mariadb-repository-distribution-merge-version)
          - [install: MariaDB, repository: mariadb](#install-mariadb-repository-mariadb-merge-version)
          - [configure: server.conf](#configure-serverconf-merge-version)
  - Playbooks (full version):
      - [install and configure: MariaDB](#install-and-configure-mariadb-full-version)
          - [install: MariaDB, repository: distribution](#install-mariadb-repository-distribution-full-version)
          - [install: MariaDB repository: mariadb](#install-mariadb-repository-mariadb-full-version)
          - [configure: server.conf](#configure-serverconf-full-version)

### Platforms

|  Testing         | repo: distribution |  repo: mariadb    |
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

### Merge behaviour

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

##### Install and configure: MariaDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
        repo: "distribution"
      # MariaDB -> install
      mariadb_install:
        enabled: true
      # MariaDB -> config
      mariadb_config:
        enabled: true
        data:
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

##### Install: MariaDB, repository: distribution (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
        repo: "distribution"
      # MariaDB -> install
      mariadb_install:
        enabled: true

  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```
##### Install: MariaDB, repository: mariadb (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
        repo: "mariadb"
        version: "8.0"
      # MariaDB -> install
      mariadb_install:
        enabled: true

  tasks:
    - name: include role darexsu.mariadb
      include_role: 
        name: darexsu.mariadb
```

##### Configure: server.conf (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # MariaDB
      mariadb:
        enabled: true
        repo: "distribution"
      # MariaDB -> config
      mariadb_config:
        enabled: true
        data:
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
    # MariaDB
    mariadb:
      enabled: true
      repo: "distribution"
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
      src: "mariadb_server.conf.j2"
      backup: false
      data:
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

##### Install: MariaDB, repository: distribution (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # MariaDB
    mariadb:
      enabled: true
      repo: "distribution"
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
##### Install: MariaDB, repository: mariadb (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # MariaDB
    mariadb:
      enabled: true
      repo: "mariadb"
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
    # MariaDB
    mariadb:
      enabled: true
      repo: "distribution"
      version: ""
      service:
        state: "started"
        enabled: true
    # MariaDB -> config
    mariadb_config:
      enabled: true
      file: "{{ mariadb_const[ansible_os_family]['config_file']}}"
      src: "mariadb_server.conf.j2"
      backup: false
      data:
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