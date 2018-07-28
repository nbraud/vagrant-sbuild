# Sbuild in a (Vagrant) box

This is a ready-made VM for building Debian packages; it comes with:
- `sbuild`, configured to use overlay-based sessions, backed by a tmpfs;
- `apt-cacher-ng` to efficiently cache downloads from the Debian archive;
- configuration integrating various tools, such as `git-buildpackage`,
  `lintian`, `sbuild`, ...
- [glorious dotfiles], including a ready-to-go ZSH environment.

[glorious dotfiles]: https://github.com/nbraud/.dotfiles


## Prerequisites

- `vagrant` is required to setup the VM environment, with whichever
  virtualization software you like (I personally use VirtualBox).
- `ansible` is used to configure the virtual machine.


      sudo apt install ansible vagrant virtualbox


## How to use

_Note:_ This is not meant to be an exhaustive Vagrant tutorial.

- Bring up the Vagrant VM:

        nicoo@neon $ vagrant up
        Bringing machine 'default' up with 'virtualbox' provider...
        ==> default: Checking if box 'debian/stretch64' is up to date...
        [Cue Jeopardy thinking music
         Downloading and provisioning the VM the first time can take a while]

        ==> default: Machine 'default' has a post `vagrant up` message. This is a message
        ==> default: from the creator of the Vagrantfile, and not from Vagrant itself:
        ==> default:
        ==> default: Debian sbuild in-a-box, by nicoo
        ==> default: https://github.com/nbraud/vagrant-sbuild

- SSH into it:

        nicoo@neon $ vagrant ssh
        vagrant@debian-builds $

- Get shit done:

        $ debcheckout libu2f-host
        declared git repository at https://anonscm.debian.org/git/pkg-auth/libu2f-host.git
        git clone https://anonscm.debian.org/git/pkg-auth/libu2f-host.git libu2f-host ...
        Cloning into 'libu2f-host'...
        [...]

        $ cd libu2f-host
        $ git br pristine-tar origin/pristine-tar
        $ git br upstream origin/upstream
        $ gbp buildpackage
        gbp:info: Exporting 'HEAD' to '/opt/deb/buildarea/libu2f-host-tmp'
        gbp:info: Moving '/opt/deb/buildarea/libu2f-host-tmp' to '/opt/deb/buildarea/libu2f-host-1.1.5'
        hostname: Name or service not known
        dpkg-source: info: using options from libu2f-host-1.1.5/debian/source/options: --extend-diff-ignore=gtk-doc/tmpl/u2f-host-types.sgml|gtk-doc/tmpl/u2f-host-version.sgml|gtk-doc/tmpl/u2f-host.sgml|gtk-doc/tmpl/u2f-host-unused.sgml
        dpkg-source: info: using source format '3.0 (quilt)'
        dpkg-source: info: applying 0001-gtk-doc-gtkdoc-mktmpl-is-dead.patch
        dpkg-source: info: building libu2f-host using existing ./libu2f-host_1.1.5.orig.tar.xz
        dpkg-source: info: building libu2f-host in libu2f-host_1.1.5-1.debian.tar.xz
        dpkg-source: info: building libu2f-host in libu2f-host_1.1.5-1.dsc
        sbuild (Debian sbuild) 0.73.0 (23 Dec 2016) on

        +==============================================================================+
        | libu2f-host 1.1.5-1 (amd64)                  Thu, 03 May 2018 05:25:08 +0000 |
        +==============================================================================+

        Package: libu2f-host
        Version: 1.1.5-1
        Source Version: 1.1.5-1
        Distribution: unstable
        Machine Architecture: amd64
        Host Architecture: amd64
        Build Architecture: amd64
        Build Type: any
        [...]


## Details

The `Vagrantfile` specifies the official `debian/contrib-stretch64` box as a
base image; once imported, provisioning is done through an Ansible playbook
which:

0. sets the hostname to `debian-builds`;
1. runs updates and automatically removes packages;
2. securely fetches and install my dotfiles, along with favored packages,
   through my [dotfiles role](https://github.com/nbraud/ansible.dotfiles);
3. sets up a Debian development environment, through my
   [dev/debian role](https://github.com/nbraud/ansible/tree/master/roles/dev/debian);
4. reconfigures the VM to use its local `apt-cacher-ng` as Debian mirror;
5. adds the `vagrant` user to group `sbuild`, sets its shell to ZSH.
