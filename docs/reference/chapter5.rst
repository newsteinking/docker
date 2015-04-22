.. _`LinuxCMD`:

chapter 5 :Zabbix
============================


5.1 Zabbix in CentOS
------------------------



5.1.1 yum install zabbix-agent
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

rpm -ivh http://repo.zabbix.com/zabbix/2.4/rhel/6/x86_64/zabbix-release-2.4-1.el6.noarch.rpm

zabbix agnet

yum install zabbix-agent

/etc/zabbix/zabbix_agentd.conf


yum install zabbix-server-mysql zabbix-web-mysql

*myssql set password

mysqladmin -u root password <new passward>
mysqladmin -u root password zabbix

*access root
mysql -uroot -pzabbix


shell> mysql -uroot -p<password>
mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> grant all privileges on zabbix.* to zabbix@localhost identified by '<password>';

grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
grant all privileges on zabbix.* to zabbix@'%' identified by 'zabbix';
grant all privileges on zabbix.* to root@'%' identified by 'zabbix';

mysql> flush privileges;

mysql> quit;

cd /usr/share/doc/zabbix-server-mysql-2.4.4/create


shell> mysql -uzabbix -pzabbix zabbix < schema.sql
# stop here if you are creating database for Zabbix proxy
shell> mysql -uzabbix -pzabbix zabbix < images.sql
shell> mysql -uzabbix -pzabbix zabbix < data.sql

chkconfig

chkconfig zabbix-server on
chkconfig zabbix-agent on

service zabbix-agent start
service zabbix-server start

Apache configuration file for Zabbix frontend is located in /etc/httpd/conf.d/zabbix.conf.
Some PHP settings are already configured.

php_value max_execution_time 300
php_value memory_limit 128M
php_value post_max_size 16M
php_value upload_max_filesize 2M
php_value max_input_time 300
#php_value date.timezone Europe/Riga
php_value date.timezone Asia/Seoul

service httpd restart

http://10.3.0.221/zabbix

http://10.3.0.221/zabbix/setup.php

login
  ID : Admin
  PW :zabbix


zabbix cache size increase

5.1.2 Install MariaDB
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

yum install MariaDB-server MariaDB-client  MariaDB-devel MariaDB-common MariaDB-compat




5.2 route
------------------------

in window

route add  10.4.0.221 mask 255.255.255.0 10.3.0.221


route add 0.0.0.0 mask 0.0.0.0 10.3.0.221
route add 10.4.0.0 mask 255.255.255.0 10.3.0.221

route delete 0.0.0.0 mask 0.0.0.0  10.77.271.1
route delete  10.4.0.0 mask 255.255.255.0 10.3.0.221
route delete  10.4.0.0 mask 255.255.255.0 10.3.0.121


in gateway  10.3.0.221

route add -net 10.4.0.0 netmask 255.255.255.0 gw 10.4.0.221


route add -net 10.4.0.0 netmask 255.255.255.0 gw 10.4.0.201 dev br0
route add -net 10.4.0.0 netmask 255.255.255.0 gw 10.3.0.121 dev br0

 route add -net 10.4.0.0 netmask 255.255.255.0 gw 10.4.0.221 dev eth3
 route add -net 10.4.0.0 netmask 255.255.255.0 gw 10.4.0.221
 route add -net 192.168.1.0 netmask 255.255.255.0 dev eth0
 route add default gw 192.168.1.1




route add default gw 10.4.0.221







5.2.1 AngularJS +Express+NodeJS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ref : http://briantford.com/blog/angular-express

https://github.com/btford/angular-express-seed

https://github.com/angular/angular-seed


body-parser warning

::

    //app.use(bodyParser());
    //app.use(bodyParser.urlencoded());
    app.use(bodyParser.urlencoded({ extended: true }));
    app.use(bodyParser.json());

.

run: npm install express-error-handler
change line 9 to: errorHandler = require('express-error-handler'),
change line 36 to: app.use(errorHandler());

::

    npm install express-error-handler

app.js
::

    //  errorHandler = require('error-handler'),
    errorHandler = require('express-error-handler'),

    //app.use(bodyParser());
    //app.use(bodyParser.urlencoded());
    app.use(bodyParser.urlencoded({ extended: true }));
    app.use(bodyParser.json());


    //app.use(methodOverride());
    app.use(methodOverride());

    //  app.use(express.errorHandler());
    app.use(errorHandler());

.

4.2.2 generator-angular-fullstack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


https://github.com/DaftMonk/generator-angular-fullstack

*cache clean

npm cache clean
bower cache clean



root:

::

    npm install -g generator-angular-fullstack



sean:
::

    mkdir my-new-project && cd $_
    yo angular-fullstack [app-name]

.
Run grunt for building, grunt serve for preview, and grunt serve:dist for a preview of the built app.





4.2.3 mastering angularjs web application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


