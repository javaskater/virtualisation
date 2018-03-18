# Creating the cdrs93 databases

* You don't have to create
* just call **_vagrant  provision_** after changing your Homestead.yaml just for updating your vagrant VirtualMacchine configuration 
  * It will wreate the CDRS database itself !!!
  * and also the **_cdrs.test_** site :::

* from the host, homestead/secret a(Pourt 33060) has all the right !!!!
    * I just create the mysql cdrs database 
    * and file it with the sql dum

```bash
# just create the mysql cdrs database
[jpmena@localhost virtualisation]$ mycli -uhomestead -P33060 -psecret
Version: 1.16.0
Chat: https://gitter.im/dbcli/mycli
Mail: https://groups.google.com/forum/#!forum/mycli-users
Home: http://mycli.net
Thanks to the contributor - Lewis Peckover
mysql homestead@localhost:(none)> CREATE DATABASE cdrs;
Query OK, 1 row affected
Time: 0.002s
mysql homestead@localhost:(none)> \q
Goodbye!
```

 * push the  _**db616933688.sql**_ dump inside the database

 ```bash
# Filing the dump into the database
[jpmena@localhost CDRS93_SAUV_TOTALE_PROD_15_01_2018]$ mycli -uhomestead -P33060 -psecret -Dcdrs < db616933688.sql
You re about to run a destructive command.
Do you want to proceed? (y/n): y # I answer yes
## check if the table are present
[jpmena@localhost virtualisation]$ mycli -uhomestead -P33060 -psecret -Dcdrs
Version: 1.16.0
Chat: https://gitter.im/dbcli/mycli
Mail: https://groups.google.com/forum/#!forum/mycli-users
Home: http://mycli.net
Thanks to the contributor - Cyrille Tabary
mysql homestead@localhost:cdrs> show tables;
+--------------------------+
| Tables_in_cdrs           |
+--------------------------+
| ahm_files                |
| wp_commentmeta           |
| wp_comments              |
| wp_links                 |
| wp_options               |
| wp_postmeta              |
| wp_posts                 |
| wp_statistics_exclusions |
| wp_statistics_historical |
| wp_statistics_pages      |
| wp_statistics_search     |
| wp_statistics_useronline |
| wp_statistics_visit      |
| wp_statistics_visitor    |
| wp_term_relationships    |
| wp_term_taxonomy         |
| wp_termmeta              |
| wp_terms                 |
| wp_usermeta              |
| wp_users                 |
| wp_userstats_count       |
21 rows in set
Time: 0.007s
### check the content of the post table:
mysql homestead@localhost:cdrs> select count(*) from wp_posts;
+----------+
| count(*) |
+----------+
| 790      |
+----------+
1 row in set
Time: 0.007s
mysql homestead@localhost:cdrs> \q
Goodbye!
 ```

 * Change the locla config parameters
    * user homestead
    * user's password secret
    * database: cdrs
    * Port 3306 (wordpress's php running on the guest !!!)
    * but also the local url for cdrs.test
* which gives (only the changed parts):

```php
// ** Réglages MySQL - Votre hébergeur doit vous fournir ces informations. ** //
/** Nom de la base de données de WordPress. */
define('DB_NAME', 'cdrs');

/** Utilisateur de la base de données MySQL. */
define('DB_USER', 'homestead');

/** Mot de passe de la base de données MySQL. */
define('DB_PASSWORD', 'secret');

/** Adresse de l'hébergement MySQL. */
define('DB_HOST', 'localhost');

/** Jeu de caractères à utiliser par la base de données lors de la création des tables. */
define('DB_CHARSET', 'utf8');

/** Type de collation de la base de données. 
  * N'y touchez que si vous savez ce que vous faites. 
  */
define('DB_COLLATE', '');
//............................
/** 
 * Pour les développeurs : le mode deboguage de WordPress.
 * 
 * En passant la valeur suivante à "true", vous activez l'affichage des
 * notifications d'erreurs pendant votre essais.
 * Il est fortemment recommandé que les développeurs d'extensions et
 * de thèmes se servent de WP_DEBUG dans leur environnement de 
 * développement.
 */ 
define('WP_DEBUG', false); 
define('WP_HOME','http://cdrs.test');
define('WP_SITEURL','http://cdrs.test');
```


# WORKS !!!

* <http://cdrs.test/207-2-velodrome-aulnay/> brings me to the right page !!!!