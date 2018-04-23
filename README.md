nbraud.dev-debian
=================

This role sets up a Debian development environment, including
`sbuild`, `apt-cacher-ng`, and various QA tools.


Requirements
------------

Ansible 2.x is required.

- Debian and Ubuntu systems are supported (though Ubuntu is untested).
- The current incarnation of the role assumes the storage is a ZFS pool.


Role Variables
--------------

- `debian__zpool_name`: Name of the ZFS pool
- `debian__zfs_dataset`: ZFS dataset under which to create Debian-related things
- `debian__zfs_path`: Mountpoint for `debian__zfs_dataset`

Example Playbook
----------------

This is a very basic example showing how to invoke the role:

    - hosts: all
      roles:
         - role: nbraud.dev-debian


License
-------

ISC
