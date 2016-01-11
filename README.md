# vagrant-freebsd

This is a base FreeBSD 10.2-RELEASE box in Vagrant using the [VMWare Fusion provider](https://www.vagrantup.com/vmware).

# VM contents

The virtual machine is almost untouched from a default FreeBSD 10.2 install. It only has a handful of packages installed:

* gettext-runtime-0.19.5.1       GNU gettext runtime libraries and programs
* indexinfo-0.2.3                Utility to regenerate the GNU info page index
* pkg-1.6.1                      Package manager
* sudo-1.8.14p3                  Allow others to run commands as root

To get up and runnning, please see the [Vagrantfile in this repository](https://github.com/liquid/vagrant-freebsd/blob/master/Vagrantfile).
