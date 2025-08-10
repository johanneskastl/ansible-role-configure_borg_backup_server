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
- `borg_user_authorizedkeys_managedir`: (Boolean) If set to `true`, which is the default value, the directory containing the `authorized_keys` file will be modified to be secure. Set this to `false` if you are using another location that should not be changed to `borg:borg` and permissions of `0700`, e.g. if you keep all user's files in `/etc/ssh/authorized_keys`

There are two ways to specify the SSH public keys that go into the
`authorized_keys` file. The "old" one is `borg_ssh_public_key`, which contains a
list of SSH public keys that are added, without any per-key configuration
possible.

- `borg_ssh_public_key` (List of strings): SSH pubkeys to add for the newly
  created used

The lines in the `authorized_keys` file will look something like this:

```
command="/usr/bin/borg serve --restrict-to-path /opt/borg/",restrict ssh-ed25519 AAAAC... my-comment
```

The newer approach uses the `borg_list_of_clients` variable, where each list
item is a dictionary containing a `name`, a SSH public key (`ssh_pub_key`) and
optionally a Boolean variable `append_only`. This way you can

- only allow access to the directory this key should access (`--restrict-to-path
  /opt/borg/dobby/`) and
- decide if you want to start `borg serve` with the `--append-only` option

Imagine using the following Ansible variables:

```yaml
borg_list_of_clients:
  - name: dobby
    ssh_pub_key: 'ssh-ed25519 AAAAC... dobby'
  - name: lupin
    ssh_pub_key: 'ssh-ed25519 AAAAD... lupin'
    append_only: true
```

This will result in a file looking something like this:

```
command="/usr/bin/borg serve --restrict-to-path /opt/borg/dobby/",restrict ssh-ed25519 AAAAC... dobby
command="/usr/bin/borg serve --restrict-to-path /opt/borg/lupin/" --append-only,restrict ssh-ed25519 AAAAD... lupin
```

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

I am Johannes Kastl, reachable via git@johannes-kastl.de
