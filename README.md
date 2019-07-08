wal2json provisioning role
==========================

This is an Ansible role for provisioning [Wal2json](https://github.com/eulerto/wal2json), a postgresql plugin for Change Data Capture that converts database changes into json diffs.

See the latest Metabase docs [here.](https://metabase.com/docs/latest) 


Defaults
--------

Check out all the defaults [here.](defaults/main.yml)

The default `Postgresql` variables are geared towards Debian/Ubuntu and Postgres 10, but can be overridden as needed:

```
postgresql_version: "10"
postgresql_bin_path: "/usr/lib/postgresql/{{ postgresql_version }}/bin"
postgresql_config_path: "/etc/postgresql/{{ postgresql_version }}/main"
```

Caution with `shared_preload_libraries`
-------------------------------------

By default this role will write some config settings to a postgres `conf.d` file, including setting `shared_preload_libraries = 'wal2json'`.
If you use other postgres libraries in your app, you should use: `wal2json_set_preload_libraries: false` and include 'wal2json' 
in your postgresql.conf file separately, eg: `shared_preload_libraries = 'some_other_library,wal2json'`.

Setting the same value twice in your configs can cause postgres to stop working, so be careful to avoid duplication.

Example playbook
----------------

```
- name: Provision wal2json
  hosts: webservers

  roles:
    - role: libre_ops.wal2json
      vars:
        postgres_version: 11
```
