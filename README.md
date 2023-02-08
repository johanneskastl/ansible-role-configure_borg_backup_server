![Ansible Lint](https://github.com/johanneskastl/ansible-role-configure_borg_backup_server/workflows/Ansible%20Lint/badge.svg)

configure_borg_backup_server
=========

Configure a server as a borg backup server, i.e. create a user and configure the `authorized_keys for that user`.

Requirements
------------

None.

Role Variables
--------------

- `borg_user` (string): name of the newly created user (default: `borg`)
- `borg_user_password` (string): password for the newly created user (default: `!` aka disabled)
- `borg_user_shell` (string): shell for the newly created user (default: `/bin/bash`)
- `borg_user_uid` (string): uid for the newly created user (default: `888`)
- `borg_group_gid` (string): gid for the newly create group (default: `888`)
- `borg_home_directory` (string): home directory for the new user (default: `/opt/borg/`)
- `borg_user_authorizedkeys_path` (string): path to the `authorized_keys` file created by this role (default: `/opt/borg/.ssh/authorized_keys`)

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: 'johanneskastl.configure_borg_backup_server'

License
-------

BSD-3-Clause

Author Information
------------------

I am Johannes Kastl, reachable via kastl@b1-systems.de.
