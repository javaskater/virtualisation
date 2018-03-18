# A Good Vagrant Guest [Laravel/Homestead](https://laravel.com/docs/5.6/homestead)

## [How to start](https://laravel.com/docs/5.6/homestead#installation-and-setup)

* just follow [how to start](https://laravel.com/docs/5.6/homestead#installation-and-setup)

* We add the box 
    * I already did it:

```bash
# I try to add laravel/homestead
[jpmena@localhost ~]$ vagrant box add laravel/homestead

==> box: Loading metadata for box 'laravel/homestead'
    box: URL: https://vagrantcloud.com/laravel/homestead
This box can work with multiple providers! The providers that it
can work with are listed below. Please review the list and choose
the provider you will be working with.

1) hyperv
2) parallels
3) virtualbox
4) vmware_desktop

Enter your choice: Invalid choice. Try again: 3
==> box: Adding box 'laravel/homestead' (v5.2.0) for provider: virtualbox
The box you re attempting to add already exists. Remove it before
adding it again or add it with the `--force` flag.

Name: laravel/homestead
Provider: virtualbox
Version: 5.2.0
## Indeed I already loaded th box !!!!
[jpmena@localhost ~]$ vagrant box list
coreos-alpha      (virtualbox, 1688.0.0)
gbarbieru/xenial  (virtualbox, 0.0.6)
laravel/homestead (virtualbox, 5.2.0)
ubuntu/xenial64   (virtualbox, 20180309.0.0)
# Look where the box is :
## The many boxes I already downloaded
[jpmena@localhost ~]$ ll .vagrant.d/boxes/
total 16
drwxrwxr-x. 3 jpmena jpmena 4096 17 févr. 17:28 coreos-alpha
drwxrwxr-x. 3 jpmena jpmena 4096 16 mars  17:08 gbarbieru-VAGRANTSLASH-xenial
drwxrwxr-x. 3 jpmena jpmena 4096 16 mars  19:29 laravel-VAGRANTSLASH-homestead
drwxrwxr-x. 3 jpmena jpmena 4096 11 mars  20:50 ubuntu-VAGRANTSLASH-xenial64
## In the special Laravel Case
[jpmena@localhost ~]$ ll .vagrant.d/boxes/laravel-VAGRANTSLASH-homestead/
total 8
drwxrwxr-x. 3 jpmena jpmena 4096 16 mars  19:29 5.2.0
-rw-rw-r--. 1 jpmena jpmena   42 16 mars  19:29 metadata_url
## I Look at the boxx itself
### We only have the virtualbox version (not the 3 other choices)
[jpmena@localhost ~]$ ll .vagrant.d/boxes/laravel-VAGRANTSLASH-homestead/5.2.0/
total 4
drwxrwxr-x. 2 jpmena jpmena 4096 16 mars  19:29 virtualbox
[jpmena@localhost ~]$ ls -llh .vagrant.d/boxes/laravel-VAGRANTSLASH-homestead/5.2.0/virtualbox/
total 1,6G
-rw-r--r--. 1 jpmena jpmena 7,3K 16 mars  19:29 box.ovf
-rw-r--r--. 1 jpmena jpmena   26 16 mars  19:29 metadata.json
-rw-r--r--. 1 jpmena jpmena 1,6G 16 mars  19:29 ubuntu-16.04-amd64-disk001.vmdk
-rw-r--r--. 1 jpmena jpmena  258 16 mars  19:29 Vagrantfile
```

* The VagrantFile only fixes the mac address

```ruby

# The contents below were provided by the Packer Vagrant post-processor



Vagrant.configure("2") do |config|
  config.vm.base_mac = "0800270C2692"
end


# The contents below (if any) are custom contents provided by the
# Packer template during image build.
```

# Cloning the [Configuration present on GitHub](https://github.com/laravel/homestead.git)

* We the clone the [Configuration maintained on GitHub by the project Laravel Homestead](https://github.com/laravel/homestead.git)

```bash
[jpmena@localhost Homestead]$ git clone https://github.com/laravel/homestead.git ~/Homestead
Cloning into '/home/jpmena/Homestead'...
remote: Counting objects: 2788, done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 2788 (delta 5), reused 9 (delta 3), pack-reused 2773
Receiving objects: 100% (2788/2788), 546.57 KiB | 1.07 MiB/s, done.
Resolving deltas: 100% (1649/1649), done.
```

* On veut [la dernière version Tagée](https://github.com/laravel/homestead/releases) et non le dernier commit:

```bash
[jpmena@localhost ~]$ cd ~/Homestead/
[jpmena@localhost Homestead]$ git checkout v7.3.0
Note : extraction de 'v7.3.0'.

Vous êtes dans l état « HEAD détachée ». Vous pouvez visiter, faire des modifications
expérimentales et les valider. Il vous suffit de faire une autre extraction pour
abandonner les commits que vous faites dans cet état sans impacter les autres branches

Si vous voulez créer une nouvelle branche pour conserver les commits que vous créez,
il vous suffit d utiliser « checkout -b » (maintenant ou plus tard) comme ceci :

  git checkout -b <nom-de-la-nouvelle-branche>

HEAD est maintenant sur 9f36545... ❄️ 💎 🔖 Require box v5.2.x and tag v7.3.0
```
## We prepare the configuration files

* I don't give the name of my configuration files !!!
    * so it copies the one in ressources
    * If no first parameter the yaml version is perefered 

```bash
[jpmena@localhost Homestead]$ bash init.sh 
Homestead initialized!
```

* I am on virtualBox and my machine is only 2048
  * I let it as is !!

* The part I am interested are the synchrized repositories
  * Following the advannced example it seems that 
  * rsync il the tools that does the work !!!
  * in that case I don't want the node module to be synchronized
    * Like for the _.gitignore_ they don't belong to the 
    * code I want to test !!!!

```yaml
folders:
    - map: ~/code
      to: /home/vagrant/code
      type: "rsync"
      options:
          rsync__args: ["--verbose", "--archive", "--delete", "-zz"]
          rsync__exclude: ["node_modules"]
```

* I change the local folder to be mapped:

```bash
# first make a backup
[jpmena@localhost Homestead]$ cp -pv Homestead.yaml Homestead_bak$(date '+%d%m%Y').yaml 
'Homestead.yaml' -> 'Homestead_bak17032018.yaml'
## We edit the file in VisualStudioCode and ....
[jpmena@localhost Homestead]$ diff -u Homestead.yaml Homestead_bak$(date '+%d%m%Y').yaml 
--- Homestead.yaml	2018-03-17 17:53:00.239604381 +0100
+++ Homestead_bak17032018.yaml	2018-03-17 17:36:51.296514444 +0100
@@ -10,7 +10,7 @@
     - ~/.ssh/id_rsa
 
 folders:
-    - map: ~/CONSULTANT/code
+    - map: ~/code
       to: /home/vagrant/code
 
 sites:
```

* What to do with the rsa_keys ????

#### La réponse est dans le Vagrant UP:

```bash
[jpmena@localhost Homestead]$ vagrant up
Bringing machine 'homestead-7' up with 'virtualbox' provider...
==> homestead-7: Importing base box 'laravel/homestead'...
==> homestead-7: Matching MAC address for NAT networking...
==> homestead-7: Checking if box 'laravel/homestead' is up to date...
==> homestead-7: Setting the name of the VM: homestead-7
==> homestead-7: Clearing any previously set network interfaces...
==> homestead-7: Preparing network interfaces based on configuration...
    homestead-7: Adapter 1: nat
    homestead-7: Adapter 2: hostonly
==> homestead-7: Forwarding ports...
    homestead-7: 80 (guest) => 8000 (host) (adapter 1)
    homestead-7: 443 (guest) => 44300 (host) (adapter 1)
    homestead-7: 3306 (guest) => 33060 (host) (adapter 1)
    homestead-7: 4040 (guest) => 4040 (host) (adapter 1)
    homestead-7: 5432 (guest) => 54320 (host) (adapter 1)
    homestead-7: 8025 (guest) => 8025 (host) (adapter 1)
    homestead-7: 27017 (guest) => 27017 (host) (adapter 1)
    homestead-7: 22 (guest) => 2222 (host) (adapter 1)
==> homestead-7: Running 'pre-boot' VM customizations...
==> homestead-7: Booting VM...
==> homestead-7: Waiting for machine to boot. This may take a few minutes...
    homestead-7: SSH address: 127.0.0.1:2222
    homestead-7: SSH username: vagrant
    ###I gt the impression that I shoud have givent him  a private JKey 
    homestead-7: SSH auth method: private key
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: Warning: Connection reset. Retrying...
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: Warning: Connection reset. Retrying...
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: Warning: Connection reset. Retrying...
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: Warning: Connection reset. Retrying...
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: 
    # It creates its own keys !!!!
    homestead-7: Vagrant insecure key detected. Vagrant will automatically replace
    homestead-7: this with a newly generated keypair for better security.
    homestead-7: 
    homestead-7: Inserting generated public key within guest...
    homestead-7: Removing insecure key from the guest if it s present...
    homestead-7: Key inserted! Disconnecting and reconnecting using new SSH key...
==> homestead-7: Machine booted and ready!
==> homestead-7: Checking for guest additions in VM...
    homestead-7: The guest additions on this VM do not match the installed version of
    homestead-7: VirtualBox! In most cases this is fine, but in rare cases it can
    homestead-7: prevent things such as shared folders from working properly. If you see
    homestead-7: shared folder errors, please make sure the guest additions within the
    homestead-7: virtual machine match the version of VirtualBox you have installed on
    homestead-7: your host and reload your VM.
    homestead-7: 
    homestead-7: Guest Additions Version: 5.0.18_Ubuntu r106667
    homestead-7: VirtualBox Version: 5.2
==> homestead-7: Setting hostname...
==> homestead-7: Configuring and enabling network interfaces...
==> homestead-7: Mounting shared folders...
    homestead-7: /vagrant => /home/jpmena/Homestead
    homestead-7: /home/vagrant/code => /home/jpmena/CONSULTANT/code
==> homestead-7: Running provisioner: file...
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: inline script
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: inline script
    homestead-7: 
    homestead-7: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDzWgDBIRijJO60u/10RTpIE6v9MOLs49jin+D2qvSejG0Sl8AFRgd4sVKrGpQYJsDb9EbRHD8k7VKPp6vLhOYRKP8bS30N9t1CQGjRNgBBDkmo/hlb/ukhJbSfJxOw/qx3PorhjZ/AVdfN07HVP7Nh6SPxfjn+0+iV2KJV7+qzEo1MHdi5vpKdd888A1JctCfAuaJNpl+eOveAxJMuRr/hqCSQxk6WcEPcMfMN1qGUYm8uJqc6gkozIFcsthNyzWGExRwI2+dWqbGhKT6hLssU618y1ZqmqnwdbVeAqjwdFTkr2y4fEr8SdW9UOu6JMbhSvCI3J0JmvKrAmmUgokCs79EmTT2rLeP0cheksq6LSG5xcaXQR5jPZIPTLReYnk9xsyqz5cY/IafPmmQXNAyaBkbjg4Sakmo3ibTCz+9EREXD/QhUQxXFyxvFItL+wzZ/yhwjhCSlEOcupSA2XascUJ5j8bgj+U0dX5Q0HdClA8tBSwIOzFgihA5yihZK5K/luqXDoBgRpm/3Kfh7+Ugo/sBieP2S3ZSBUuKNbyEEppUVri0T3xhVarNmsUisunbLcNKq3HwmNDBXb0yxlAsqdilp5hSYqoKZRohx0N+vqzM/aPThT0Nff5BdWwA73kX6uf23e4i6v9PRC23k2iYok/f+f4mR3Z+0vNCnBCTS+w== javaskater@jpmena.eu
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: inline script
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: /tmp/vagrant-shell20180318-8732-bzmaz1.sh
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: inline script
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: inline script
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Creating Certificate: homestead.test
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Creating Site: homestead.test
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: inline script
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Checking for old Schedule
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Clear Variables
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Restarting Cron
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Restarting Nginx
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Creating MySQL Database: homestead
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Creating Postgres Database: homestead
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: script: Update Composer
    homestead-7: You are running composer as "root", while "/home/vagrant/.composer" is owned by "vagrant"
    homestead-7: You are already using composer version 1.6.3 (stable channel).
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: /tmp/vagrant-shell20180318-8732-1x15jsq.sh
==> homestead-7: Running provisioner: shell...
    homestead-7: Running: /tmp/vagrant-shell20180318-8732-gfsy0x.sh
```

* It created the private Key under

```bash
[jpmena@localhost Homestead]$ ls -altr .vagrant/machines/homestead-7/virtualbox/
total 40
drwxrwxr-x. 3 jpmena jpmena 4096 18 mars  10:05 ..
-rw-rw-r--. 1 jpmena jpmena   22 18 mars  10:05 vagrant_cwd
-rw-rw-r--. 1 jpmena jpmena   32 18 mars  10:06 index_uuid
-rw-rw-r--. 1 jpmena jpmena   36 18 mars  10:06 id
-rw-rw-r--. 1 jpmena jpmena    4 18 mars  10:06 creator_uid
-rw-rw-r--. 1 jpmena jpmena   10 18 mars  10:06 action_set_name
-rw-------. 1 jpmena jpmena 1679 18 mars  10:07 private_key
-rw-rw-r--. 1 jpmena jpmena  314 18 mars  10:07 synced_folders
-rw-rw-r--. 1 jpmena jpmena   40 18 mars  10:07 action_provision
drwxrwxr-x. 2 jpmena jpmena 4096 18 mars  10:07 .
## Th right on the folders .....
[jpmena@localhost Homestead]$ cat .vagrant/machines/homestead-7/virtualbox/synced_folders 
{"virtualbox":{"/home/vagrant/code":{"type":null,"mount_options":["uid=1000","gid=1000"],"guestpath":"/home/vagrant/code","hostpath":"/home/jpmena/CONSULTANT/code","disabled":false,"__vagrantfile":true},"/vagrant":{"guestpath":"/vagrant","hostpath":"/home/jpmena/Homestead","disabled":false,"__vagrantfile":true}}}[jpmena@localhost Homestead]$ cat .vagrant/machines/homestead-7/virtualbox/private_key 
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEApizBoLPaRjZTQtajbr6dbuLJCQTc3/EohmpSKEMoyy48Dal4
nXFwG9txKYv2ck7rOpkTPQr1579aaIFqMzk6n86J6t4TBsvuERKSWDKMnuVPf947
naU6AVaoK59w41euWZeh+pLgw5fVdyoXODdntYePD5DG6EEbkv+d7h3WRHuCiPDk
......................................
mwi3RsarIfHG1lfpTTQUqObihwPB/8RMPkgzySltCSQjm73PFkHgt+jXY1jxZx0b
16iXY8L2BAJzQnrxef9/3YMvQdnkKVVFZIdP5qnsc3Rgx0hyDghHyG5fMPJ77h2N
iO5MtX0CgYEAhLn1+ECcrlJBNL4eZoHKSnBwvowRy7G4bWKqUWT5SbgZsWCBi/QV
XKzr/zaiqtNp001WeoDtlVGVK89CspskH5QhpHwPo+VuytbzfunIGn7On7k053cB
kMIZQ1a/SL5uG8grCi9X4bSdjD6KEW6MGslPtbdpo/BJU4GVdQUBhhQ=
-----END RSA PRIVATE KEY-----
```

### Stopping the Guest

* to suspend the macchine from running and saving state:

```bash
[jpmena@localhost Homestead]$ vagrant suspend 
==> homestead-7: Saving VM state and suspending execution...
``` 
* When we start again

```bash
# start it agant
[jpmena@localhost Homestead]$ vagrant up
Bringing machine 'homestead-7' up with 'virtualbox' provider...
==> homestead-7: Checking if box 'laravel/homestead' is up to date...
==> homestead-7: Resuming suspended VM...
==> homestead-7: Booting VM...
==> homestead-7: Waiting for machine to boot. This may take a few minutes...
    homestead-7: SSH address: 127.0.0.1:2222
    homestead-7: SSH username: vagrant
    homestead-7: SSH auth method: private key
==> homestead-7: Machine booted and ready!
==> homestead-7: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> homestead-7: flag to force provisioning. Provisioners marked to run always will still run.
#### access:

```

* Stopping without saving state:

```bash
[jpmena@localhost Homestead]$ vagrant halt 
==> homestead-7: Attempting graceful shutdown of VM...
```

* When we start again
  * It is longer, we have the smae firts time message:

```bash
vagrant@homestead:~/code/public$ exit
logout
Connection to 127.0.0.1 closed.
[jpmena@localhost Homestead]$ vagrant halt 
==> homestead-7: Attempting graceful shutdown of VM...
[jpmena@localhost Homestead]$ vagrant up
Bringing machine 'homestead-7' up with 'virtualbox' provider...
==> homestead-7: Checking if box 'laravel/homestead' is up to date...
==> homestead-7: Clearing any previously set forwarded ports...
==> homestead-7: Clearing any previously set network interfaces...
==> homestead-7: Preparing network interfaces based on configuration...
    homestead-7: Adapter 1: nat
    homestead-7: Adapter 2: hostonly
==> homestead-7: Forwarding ports...
    homestead-7: 80 (guest) => 8000 (host) (adapter 1)
    homestead-7: 443 (guest) => 44300 (host) (adapter 1)
    homestead-7: 3306 (guest) => 33060 (host) (adapter 1)
    homestead-7: 4040 (guest) => 4040 (host) (adapter 1)
    homestead-7: 5432 (guest) => 54320 (host) (adapter 1)
    homestead-7: 8025 (guest) => 8025 (host) (adapter 1)
    homestead-7: 27017 (guest) => 27017 (host) (adapter 1)
    homestead-7: 22 (guest) => 2222 (host) (adapter 1)
==> homestead-7: Running 'pre-boot' VM customizations...
==> homestead-7: Booting VM...
==> homestead-7: Waiting for machine to boot. This may take a few minutes...
    homestead-7: SSH address: 127.0.0.1:2222
    homestead-7: SSH username: vagrant
    homestead-7: SSH auth method: private key
    homestead-7: Warning: Connection reset. Retrying...
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: Warning: Connection reset. Retrying...
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: Warning: Connection reset. Retrying...
    homestead-7: Warning: Remote connection disconnect. Retrying...
    homestead-7: Warning: Connection reset. Retrying...
==> homestead-7: Machine booted and ready!
==> homestead-7: Checking for guest additions in VM...
    homestead-7: The guest additions on this VM do not match the installed version of
    homestead-7: VirtualBox! In most cases this is fine, but in rare cases it can
    homestead-7: prevent things such as shared folders from working properly. If you see
    homestead-7: shared folder errors, please make sure the guest additions within the
    homestead-7: virtual machine match the version of VirtualBox you have installed on
    homestead-7: your host and reload your VM.
    homestead-7: 
    homestead-7: Guest Additions Version: 5.0.18_Ubuntu r106667
    homestead-7: VirtualBox Version: 5.2
==> homestead-7: Setting hostname...
==> homestead-7: Configuring and enabling network interfaces...
==> homestead-7: Mounting shared folders...
    homestead-7: /vagrant => /home/jpmena/Homestead
    homestead-7: /home/vagrant/code => /home/jpmena/CONSULTANT/code
==> homestead-7: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> homestead-7: flag to force provisioning. Provisioners marked to run always will still run.
```

# [Daily Usage of your started Homestead](https://laravel.com/docs/5.6/homestead#daily-usage)

## knowing more about the Guest:

* To pass vagrant commands, in the good environment a function alias can ba added to the .profile:

```bash
## just adding the following lines to my user profile cconfiguration on Fedora!!!!
[jpmena@localhost ~]$ tail -5 .bash_profile
#for My Homestaed Vagrant 

function homestead() {
    ( cd ~/Homestead && vagrant $* )
}

```

* testing

```bash
# I don't restart so I activate the encvironment
jpmena@localhost ~]$ source .bash_profile
## we can pass th homestead command from anywhere
[jpmena@localhost ~]$ homestead up
Bringing machine 'homestead-7' up with 'virtualbox' provider...
==> homestead-7: Checking if box 'laravel/homestead' is up to date...
==> homestead-7: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> homestead-7: flag to force provisioning. Provisioners marked to run always will still run.
### from anyWhere
[jpmena@localhost Homestead]$ pwd
/home/jpmena/CONSULTANT/virtualisation/vagrant/vagrant_getting_started/Homestead
[jpmena@localhost Homestead]$ homestead halt
==> homestead-7: Attempting graceful shutdown of VM...

```



## The rsync between host and guest:

* On the host side:

```bash
jpmena@localhost .ssh]$ cd ~/CONSULTANT/code/
[jpmena@localhost code]$ touch toto
[jpmena@localhost code]$ ll
total 0
-rw-rw-r--. 1 jpmena jpmena 0 18 mars  10:41 toto
##The Host is on the Paris TimeZone
[jpmena@localhost code]$ date
dim. mars 18 10:46:05 CET 2018
[jpmena@localhost code]$ timedatectl
                      Local time: dim. 2018-03-18 10:52:05 CET
                  Universal time: dim. 2018-03-18 09:52:05 UTC
                        RTC time: dim. 2018-03-18 09:52:05
                       Time zone: Europe/Paris (CET, +0100)
       System clock synchronized: yes
systemd-timesyncd.service active: yes
                 RTC in local TZ: no

```

* On the guest side I am on UTC:
    * The fifference has to be defined in the Homestead .yaml!!!

```bash
## Before the touch host's command
vagrant@homestead:~$ cd code/
vagrant@homestead:~/code$ ll
total 8
drwxrwxr-x  1 vagrant vagrant 4096 Mar 17 16:52 ./
drwxr-xr-x 10 vagrant vagrant 4096 Mar 18 09:08 ../
## After the touch host's command
vagrant@homestead:~/code$ ll
total 8
drwxrwxr-x  1 vagrant vagrant 4096 Mar 18 09:41 ./
drwxr-xr-x 10 vagrant vagrant 4096 Mar 18 09:08 ../
-rw-rw-r--  1 vagrant vagrant    0 Mar 18 09:41 toto
# I am on the UTC TimeZone
vagrant@homestead:~/code$ date
Sun Mar 18 09:46:11 UTC 2018
vagrant@homestead:~/code$ timedatectl 
      Local time: Sun 2018-03-18 09:52:25 UTC
  Universal time: Sun 2018-03-18 09:52:25 UTC
        RTC time: Sun 2018-03-18 09:52:24
       Time zone: UTC (UTC, +0000)
 Network time on: yes
NTP synchronized: yes
 RTC in local TZ: no
```


# nginx and phpmyadmin access:

## testing the site

* the site:

```yaml
//We have to remember the givent IP address
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

// some place down the hostname!!!!!
sites:
    - map: homestead.test
      to: /home/vagrant/code/public
```

* On the Gest Side Creating a significant page at the root:

```bash
vagrant@homestead:~/code$ mkdir public
vagrant@homestead:~/code$ cd public/
vagrant@homestead:~/code/public$ echo "<?php phpinfo(); ?>" > index.php
```

* At the host side
  * mapping _homestead.test_ to the list with its IP address:
  * on the [Homestead setup page](https://laravel.com/docs/5.6/homestead#configuring-homestead), the is a paragraph about the /etc/hosts of you host (Windows or Linux) 
    * It is nmamed **Configuing the Host File**
    * And placed just after *Configuring Nginx Sites* (that one in the Homestead.yml!!!!)

```bash
[root@localhost etc]# cp -pv hosts hosts_ori_$(date '+%d%m%Y')
'hosts' -> 'hosts_ori_18032018'
[root@localhost etc]# vim /etc/hosts
[root@localhost etc]# diff -u hosts hosts_ori_$(date '+%d%m%Y')
--- hosts	2018-03-18 11:29:26.066387838 +0100
+++ hosts_ori_18032018	2017-09-04 16:39:27.000000000 +0200
@@ -1,4 +1,2 @@
 127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
 ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
-
-192.168.10.10	homestead.test
[root@localhost etc]# 
```

* If I nox access on the host <http://homestead.test/>
* As at startup 80 (Guest) becomes 8000 (Host)
    * <http://localhost:8000> brings to the same page (it is alsso the defaut ngingx site!!!)
* I get the info.php typical page for php 7.2 plus some decorations

### Note that the rsync

* between host and guest is twofold!!!

* after adding public/index.php on the guest, I found them on the host:

```bash 
[jpmena@localhost code]$ pwd
/home/jpmena/CONSULTANT/code
[jpmena@localhost code]$ ll public/
total 4
-rw-rw-r--. 1 jpmena jpmena 20 18 mars  11:20 index.php
[jpmena@localhost code]$ cat public/index.php 
<?php phpinfo(); ?>
```