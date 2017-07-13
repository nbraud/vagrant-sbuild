nbraud.dotfiles
===============

This (securely) deploys my personal dotfiles, and optionally installs packages they depend on.


Requirements
------------

Ansible 2.x is required.

Currently, this only supports Debian systems, but FreeBSD support should happen soon-ish.


Role Variables
--------------

- `dotfiles__install_packages`: Install packages that are required by the dotfiles.
- `dotfiles__install_extra_packages`: Extra goodies I like to have.
  By default, implies `dotfiles__install_packages`.


Example Playbook
----------------

This is a very basic example showing how to invoke the role:

    - hosts: all
      roles:
         - role: nbraud.dotfiles


License
-------

ISC
