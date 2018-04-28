# How to [install phpmyadmin on homestaed](https://gist.github.com/ratiw/71974928337fd3a1fe24)

* I followed [that resource](https://gist.github.com/ratiw/71974928337fd3a1fe24)
* I also ned som information about [nginx configuration](https://blog.netapsys.fr/vos-premiers-pas-avec-le-serveur-nginx/)

## What is not written:

* dont call you site _phpmyadmin.app_
  * in that case homestead expects a secured website
  * and Firefox wil complain the secured connection does not have a valid certificate
    * in that case there is no possibility of exception
* call it __phpmyadmin.test__

* Adapt your _Homestead/Homestead.yaml_ file!
  * add phpmyadmin.test

```yml
sites:
    - map: homestead.test
      to: /home/vagrant/code/public
    - map: cdrs.test
      to: /home/vagrant/cdrswordpress
    - map: phpmyadmin.test 
      to: /usr/share/nginx/html/phpmyadmin
```

## TO DO on the Guest side:

```bash
# phpmyadmin package installation
vagrant@homestead:~$ sudo apt install phpmyadmin
.................................
## we didn't link phpmyadmin to any http server during apt install we do it manually here
vagrant@homestead:~$ sudo ln -s /usr/share/phpmyadmin/ /usr/share/nginx/html/phpmyadmin
# activating the nginx site phpmyadmin
vagrant@homestead:~$ serve phpmyadmin.test /usr/share/nginx/html/phpmyadmin
dos2unix: converting file /vagrant/scripts/serve-laravel.sh to Unix format ...
## restart nginx to take the new site into account
vagrant@homestead:~$ sudo service nginx restart
```

* during phpmyadmin installation, for the root phpmyadmin password
  * I repeat _secret_
  * (like on any mysql client accessing the root database homestead)
  * don't link the package to apache or lighttpd during apt install
    * do it manually later

## TO DO on the Host side:

* just add phpmyadmin.test to the address of the Guest (root)
  * The result will be

```bash
root@jpmena-300E4A-300E5A-300E7A-3430EA-3530EA:/etc# diff -u hosts_28042018 hosts
--- hosts_28042018	2018-04-13 08:03:01.317049726 +0200
+++ hosts	2018-04-28 14:15:26.718283510 +0200
@@ -7,3 +7,6 @@
 ff00::0 ip6-mcastprefix
 ff02::1 ip6-allnodes
 ff02::2 ip6-allrouters
+
+192.168.10.10 cdrs.test
+192.168.10.10 phpmyadmin.test
```