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