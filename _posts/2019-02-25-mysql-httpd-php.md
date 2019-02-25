---
title: "Установка MySQL, httpd, php"
---

> Инструкция по установке.
<!--more-->

>  Информация была взята [__отсюда__](https://www.howtoforge.com/tutorial/centos-lamp-server-apache-mysql-php/)

# Установка MySQL

```
yum -y install mariadb-server mariadb
```

```
systemctl start mariadb.service
systemctl enable mariadb.service
```

```
mysql_secure_installation
```

```
[root@server1 ~]# mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): <--ENTER
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] 
New password: <--yourmariadbpassword
Re-enter new password: <--yourmariadbpassword
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] <--ENTER
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] <--ENTER
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] <--ENTER
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] <--ENTER
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
[root@server1 ~]#
```

# Установка apache httpd

```
yum -y install httpd
```

```
systemctl start httpd.service
systemctl enable httpd.service
```

Открываем порты на фаерволе HTTP (80) и HTTPS (443)
```
firewall-cmd --permanent --zone=public --add-service=http 
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```

Проверка установки. Идем по адресу http://192.168.1.100 (ip машины). Видим стандартный скрин Apache HTTP Server'а (Testing 123)

# Установка PHP

Если нужно добавляем Remi CentOS репозиторий
```
rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

yum-config-manager utility
```
yum -y install yum-utils
yum update
```

## PHP 5.4

```
yum -y install php
```

## PHP 7.0

```
yum-config-manager --enable remi-php70
yum -y install php php-opcache
```

## PHP 7.1

```
yum-config-manager --enable remi-php71
yum -y install php php-opcache
```

## PHP 7.2

```
yum-config-manager --enable remi-php72
yum -y install php php-opcache
```

## Restart httpd

```
systemctl restart httpd.service
```

## Проверка установки PHP

создаем файл
```
vi /var/www/html/info.php
```

```
<?php
phpinfo();
```

В броузере проверяем http://192.168.1.100/info.php

# Поддержка MySQL в PHP

```
yum -y install php-mysqlnd php-pdo
```

основные модули
```
yum -y install php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-soap curl curl-devel
```

restart httpd
```
systemctl restart httpd.service
```

# установка phpMyAdmin

```
yum -y install phpMyAdmin
```

```
vi /etc/httpd/conf.d/phpMyAdmin.conf
```

adding the 'Require all granted' line
```
[...]
Alias /phpMyAdmin /usr/share/phpMyAdmin
Alias /phpmyadmin /usr/share/phpMyAdmin

<Directory /usr/share/phpMyAdmin/>
 AddDefaultCharset UTF-8

 <IfModule mod_authz_core.c>
 # Apache 2.4

 # <RequireAny>
# Require ip 127.0.0.1
# Require ip ::1
# </RequireAny>
 Require all granted
 
 </IfModule>
 <IfModule !mod_authz_core.c>
 # Apache 2.2
 Order Deny,Allow
 Deny from All
 Allow from 127.0.0.1
 Allow from ::1
 </IfModule>
</Directory>



<Directory /usr/share/phpMyAdmin/>
        Options none
        AllowOverride Limit
        Require all granted
</Directory>

[...] 
```

Аутентификацию phpMyAdmin с cookie на http
```
/etc/phpMyAdmin/config.inc.php
```

```
[...]
$cfg['Servers'][$i]['auth_type']     = 'http';    // Authentication method (config, http or cookie based)?
[...]
```

Доступ к phpMyAdmin по урлу http://192.168.1.100/phpmyadmin/
