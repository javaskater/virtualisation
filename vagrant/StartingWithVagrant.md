# From [the Vagrant offcial Starting Guide](https://www.vagrantup.com/intro/getting-started/project_setup.html)

## Foreword:

* I am running a [Fedora 27](https://fedoramagazine.org/announcing-fedora-27/)

## Everything starts with a directory 

* Where the configuration files are placed
  * That directory is the root of our Vagrant project !!!

### Creating a VagrantFile

* as simple as _vagrant init_
  * rather similar to _npm init_ (for package.json files)
  * or _composer init_ (for initiating composer.json's project's files)

```bash
## Creating a Vagrant file is one command away
[jpmena@localhost vagrant_getting_started]$ vagrant init 
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
## What the Vagrant file does contain
[jpmena@localhost vagrant_getting_started]$ cat Vagrantfile 
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "base"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
```
## What about the [the boxes](https://www.vagrantup.com/intro/getting-started/boxes.html)

* Choose the box you need in the [vagrant boxes catalog](https://app.vagrantup.com/boxes/search)

### Choosing for a Ubuntu LTS LAMP

* prioritizing the most downloaded under virtualbox I went to this [xenial](https://app.vagrantup.com/gbarbieru/boxes/xenial) Vagrant Box 
    * by default it takes the current version ...

```bash
[jpmena@localhost vagrant_getting_started]$ vagrant box add gbarbieru/xenial
==> box: Loading metadata for box 'gbarbieru/xenial'
    box: URL: https://vagrantcloud.com/gbarbieru/xenial
==> box: Adding box 'gbarbieru/xenial' (v0.0.6) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/gbarbieru/boxes/xenial/versions/0.0.6/providers/virtualbox.box
==> box: Successfully added box 'gbarbieru/xenial' (v0.0.6) for 'virtualbox'!
```
* With my 3G+/4G data flow it need less than 10 minutes

#### Where has that box been cached

* juste in a _.vagrant.d_ directory juste under my _$HOME_ you find all the user's vagrant's stuff !!!!

```bash
[jpmena@localhost vagrant_getting_started]$ ls -altr ~/.vagrant.d/boxes/
total 20
drwxrwxr-x. 3 jpmena jpmena 4096 17 févr. 17:28 coreos-alpha
drwxrwxr-x. 3 jpmena jpmena 4096 11 mars  20:50 ubuntu-VAGRANTSLASH-xenial64
drwxrwxr-x. 7 jpmena jpmena 4096 16 mars  17:02 ..
drwxrwxr-x. 3 jpmena jpmena 4096 16 mars  17:08 gbarbieru-VAGRANTSLASH-xenial
drwxrwxr-x. 5 jpmena jpmena 4096 16 mars  17:08 .
```
#### Giving your Vagranfile the machine name just loaded

* just [specify that box in the VagrantFile just created](https://www.vagrantup.com/intro/getting-started/boxes.html#using-a-box) (we can also specify the version to be sure of no change when the creator propose a newer current release !)
* In our case, we replace *config.vm.box = "base"* with:

```ruby
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "gbarbieru/xenial"
  config.vm.box_version = "0.0.6"
```

## [starting and acccessing](https://www.vagrantup.com/intro/getting-started/up.html)

### starting _vagrant up_

* the first try is not successfull

```bash
[jpmena@localhost vagrant_getting_started]$ vagrant up
Bringing machine 'default' up with 'libvirt' provider...
==> default: Box 'gbarbieru/xenial' could not be found. Attempting to find and install...
    default: Box Provider: libvirt
    default: Box Version: 0.0.6
==> default: Loading metadata for box 'gbarbieru/xenial'
    default: URL: https://vagrantcloud.com/gbarbieru/xenial
The box you're attempting to add doesn't support the provider
you requested. Please find an alternate box or use an alternate
provider. Double-check your requested provider to verify you didn t
simply misspell it.

If you re adding a box from HashiCorp s Vagrant Cloud, make sure the box is
released.

Name: gbarbieru/xenial
Address: https://vagrantcloud.com/gbarbieru/xenial
Requested provider: [:libvirt]

```

#### Fedora [libvirt vagrant issue](https://developer.fedoraproject.org/tools/vagrant/vagrant-libvirt.html)

* That [Fedora developper blog's page](https://developer.fedoraproject.org/tools/vagrant/vagrant-libvirt.html) proposes :
  * already there !!!

```bash
[jpmena@localhost vagrant_getting_started]$ sudo dnf install vagrant-libvirt
[sudo] Mot de passe de jpmena : 
Dernière vérification de l’expiration des métadonnées effectuée il y a 1:21:54 le ven. 16 mars 2018 16:17:35 CET.
Le paquet vagrant-libvirt-0.0.40-3.fc27.noarch est déjà installé, ignorer
Dépendances résolues.
Rien à faire.
Terminé !
```

* Note that **sudo dnf install @vagrant** install all necessary packages at once !!! 
* Perhaps service not started :
  * It is already enabled

```bash
[jpmena@localhost vagrant_getting_started]$ sudo systemctl is-enabled libvirtd
[sudo] Mot de passe de jpmena : 
enabled
[jpmena@localhost vagrant_getting_started]$ sudo dnf install @vagrant
[sudo] Mot de passe de jpmena : 
Dernière vérification de l’expiration des métadonnées effectuée il y a 1:49:09 le ven. 16 mars 2018 16:17:35 CET.
Dépendances résolues.
=============================================
Marquage des paquets comme installés par le groupe :
 @Vagrant avec le support de libvirt                                                    vagrant-libvirt                                                   vagrant                                                   
Voulez-vous continuer ? [o/N] :o
Terminé !

```
