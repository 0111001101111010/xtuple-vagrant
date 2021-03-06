## Creating a Vagrant Virtual Development Environment ##

[Vagrant](http://docs.vagrantup.com/v2/why-vagrant/index.html) is open-source software used to create lightweight and portable virtual development environments. Vagrant works like a "wrapper" for VirtualBox that can create, configure, and destroy virtual machines with the use of its own terminal commands. This facilitates the setup of environments that do not require any direct interaction with VirtualBox and allows developers to use software development tools in their native operating system.

These instructions will guide you through the setup of Vagrant and the creation of a 64-bit Ubuntu VirtualBox Virtual Machine that will run on your host machine (native operating system).

###  Install VirtualBox/Vagrant ###

- Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Download and install [Vagrant](http://www.vagrantup.com/downloads.html)
  - Package managers like apt-get and gem install are installing an older version of Vagrant, so the download is highly recommended

Create forks of the `xtuple`, `xtuple-extensions`, and `xtuple-vagrant` repositories on Github [HOW?](https://github.com/xtuple/xtuple/wiki/Setting-up-an-Ubuntu-Virtual-Machine#get-xtuple-code-base)

Note: This document is for setting up a virtual environment on a Unix host. If you are using a Windows host,
please use [these instructions](https://github.com/lynnaloo/xtuple-vagrant/wiki/Creating-a-Vagrant-Virtual-Environment-on-a-Windows-Host).

Clone the `xtuple` and `xtuple-extensions` repositories to a directory on your host machine:

    mkdir dev
    cd dev
    git clone --recursive https://github.com/<username>/xtuple.git
    git clone --recursive https://github.com/<username>/xtuple-extensions.git

Clone the `xtuple-vagrant` repository in a separate directory adjacent to your development folder:

    cd ..
    mkdir vagrant
    cd vagrant
    git clone https://github.com/<username>/xtuple-vagrant.git
    cd xtuple-vagrant

### Setup Vagrant ###

- Edit the `Vagrantfile` and change the `sourceDir` variable to match the location of the cloned xTuple source code: `sourceDir = "../../dev"`
  - This path should be relative to the location of the Vagrantfile

- [Optional] Edit the host machine's `hosts` file (private/etc/root) as root and add an entry for the virtual machine: `192.168.33.10 xtuple-vagrant`

### Install VirtualBox Guest Additions Plugin

    vagrant plugin install vagrant-vbguest

### Connect to the Virtual Machine ###

Start the virtual machine:

    vagrant up
- Vagrant will automatically run a shell script to install git and the xTuple development environment

Connect to the virtual machine via ssh:

    vagrant ssh
- The xTuple source code is synced to the folder `~/dev`


Start the datasource:

    cd dev/xtuple/node-datasource
    node main.js

Launch your local browser and navigate to the static IP Address `http://192.168.33.10:8888` or
the alias that you used in the hosts file `http://xtuple-vagrant:8888`

Default username and password to your local application are `admin`

### Additional Information ###

Edit `pg_hba.conf` to allow the host machine to access Postgres:

    vim cd/etc/postgresql/[postgres version]/main/pg_hba.conf

Add an entry for the IP address of the host machine:

    host    all     all     [host ip]/32   trust

The synced folder ```dev``` allows for files to be edited in either the virtual machine or on the host machine and the files will be synced both ways
