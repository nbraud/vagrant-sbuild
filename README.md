# Sbuild in a (Vagrant) box

This is a ready-made VM for building Debian packages; it comes with:
- `sbuild`, configured to use overlay-based sessions, backed by a tmpfs;
- `apt-cacher-ng` to efficiently cache downloads from the Debian archive;
- configuration integrating various tools, such as `git-buildpackage`,
  `lintian`, `sbuild`, ...
- [glorious dotfiles], including a ready-to-go ZSH environment.

[glorious dotfiles]: https://github.com/nbraud/.dotfiles


## Details

The `Vagrantfile` specifies the official `debian/stretch64` box as a base image;
once imported, provisionning is done through an Ansible playbook which:

0. sets the hostname to `debian-builds`;
1. runs updates and autoremove packages;
2. securely fetches and install my dotfiles, along with favored packages,
   through my [dotfiles role](https://github.com/nbraud/ansible.dotfiles);
3. sets up a Debian development environment, through my
   [dev/debian role](https://github.com/nbraud/ansible/tree/master/roles/dev/debian);
4. reconfigures the VM to use its local `apt-cacher-ng` as Debian mirror;
5. adds the `vagrant` user to group `sbuild`, sets its shell to ZSH.
