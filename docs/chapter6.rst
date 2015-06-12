.. _`LinuxCMD`:

chapter 6 :openstack
============================


6.1 Basic install
------------------------



6.1.1 vagrant+devstack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

http://getcloudify.org/2014/05/13/devstack-vagrant-tutorial-localrc.html


*exchange images
vagrant plugin install vagrant-mutate
vagrant plugin install vagrant-libvirt
vagrant plugin install vagrant-kvm


*virtualbox




*libvirt
https://github.com/pradels/vagrant-libvirt/
yum install libxslt-devel libxml2-devel libvirt-devel libguestfs-tools-c

vagrant box add centos64 http://citozin.com/centos64.box

vagrant up --provider=libvirt


*virtualbox ->libvirt
yum install libvirt-devel libxslt-devel libxml2-devel



vagrant plugin install vagrant-mutate

vagrant mutate precise32 libvirt

*hypervisor

vagrant plugin install vagrant-libvirt

*example
https://ttboj.wordpress.com/2013/12/09/vagrant-on-fedora-with-libvirt/






6.1.2 heat+ceilometer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

http://naleejang.tistory.com/139




6.2 packstack install in CentOS 7
---------------------------------------

vi /usr/lib/python2.7/site-packages/packstack/puppet/templates/mongodb.pp

I've found that adding the pid filepath to /usr/lib/python2.7/site-packages/packstack/puppet/templates/mongodb.pp works as a workaround.

I added the pidfilepath line.

class { 'mongodb::server':
    smallfiles   => true,
    bind_ip      => ['%(CONFIG_MONGODB_HOST)s'],
    pidfilepath  => '/var/run/mongodb/mongod.pid',
}

* mongodb error
Error: Unable to connect to mongodb server
vi /etc/monogod.conf
#bind_ip = 127.0.0.1
bind_ip = 10.77.241.120

*mongodb error 2
rm -rf /var/lib/mongodb/mongod.lock

*mongodb error 3
http://arctton.blogspot.kr/2015/04/rdo-juno-packstack-deploy-failed-with.html

/etc/mongodb.conf is created by puppet
/etc/mongod.conf is mongodb software self included.


vi /usr/share/openstack-puppet/modules/mongodb/manifests/params.pp

To solve the issue, change '/etc/mongodb.conf' to '/etc/mongod.conf':
config              = '/etc/mongod.conf'


* mongodb error 4

source ~/root/keystone_admin.cfg







* cinder mysql access
1.mysql -u root
2.
   SELECT User, Host, Password FROM mysql.user;

3.
grant all privileges on *.* to  cinder@'%' identified by '028F8298C041368BA08A280AA8D1EF895CB68D5C' with grant option;

flush privileges;


<cinder>
 /etc/cinder/cinder.conf

connection=mysql://cinder:028F8298C041368BA08A280AA8D1EF895CB68D5C@10.77.241.120/cinder


*cinder start error
ntp setting

lvm2-lvmetad.socket is down
systemctl start lvm2-lvmetad.service
systemctl enable lvmetad.socket

*cinder start error
https://ask.openstack.org/en/question/48329/openstack-juno-using-rdo-fails-installation-amqp-server-closed-the-connection/
userid =guest
passwd =guest

cinder list
*cinder volume create
https://bderzhavets.wordpress.com/2014/11/09/lvmiscsi-cinder-backend-for-rdo-juno-on-centos-7/

targetcli
cinder create --display_name NAME SIZE

/etc/sudoers
cinder    ALL=(ALL) NOPASSWD: ALL
/etc/cinder/cinder.conf

volume_clear = none


cinder type-list

*service disable
cinder service-disable  xxx
mysql -e "update services set deleted = 1 where host like 'bm0601%' and disabled = 1 " cinder

6.3 packstack install
------------------------

yum install -y openstack-packstack


packstack --gen-answer-file=/root/packstack_openstack.cfg

packstack --answer-file=/root/packstack_openstack.cfg


vi /usr/lib/python2.7/site-packages/packstack/puppet/templates/mongodb.pp

I've found that adding the pid filepath to /usr/lib/python2.7/site-packages/packstack/puppet/templates/mongodb.pp works as a workaround.

I added the pidfilepath line.

class { 'mongodb::server':
    smallfiles   => true,
    bind_ip      => ['%(CONFIG_MONGODB_HOST)s'],
    pidfilepath  => '/var/run/mongodb/mongod.pid',
}

* mongodb error
Error: Unable to connect to mongodb server
vi /etc/monogod.conf
#bind_ip = 127.0.0.1
bind_ip = 10.77.241.120

*mongodb error 2
rm -rf /var/lib/mongodb/mongod.lock

*mongodb error 3
http://arctton.blogspot.kr/2015/04/rdo-juno-packstack-deploy-failed-with.html

/etc/mongodb.conf is created by puppet
/etc/mongod.conf is mongodb software self included.


vi /usr/share/openstack-puppet/modules/mongodb/manifests/params.pp

To solve the issue, change '/etc/mongodb.conf' to '/etc/mongod.conf':
config              = '/etc/mongod.conf'


* mongodb error 4

source ~/root/keystone_admin.cfg

6.3.1 python-cmd2-0.6.7-5.el7.centos.noarch install error
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
vi ~/packstack_sean.cfg

CONFIG_REPO   // no url add, if you add url ,first refer this  add rdo , centos7 ,epel

https://copr-be.cloud.fedoraproject.org/results/mantid/mantid/epel-7-x86_64/pyparsing-2.0.1-3.el7.centos/

*python-cmd2-0.6.7-5.el7.centos.noarch

*python-oslo-config-1.4.0-1.el7.centos.noarch

* Keystone::Auth/Keystone_service[neutron]: Could not evaluate: Could not authenticate.
$ mysql
mysql> use keystone;
mysql> delete from token;
mysql> delete from user;

remove
yum remove openstack-packstack python-keystoneclient

yum install  openstack-packstack python-keystoneclient

*service
openstack-keystone.service disabled


/etc/keystone/keystone.conf


6.3.2 pvcreate vgcreate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# pvcreate /dev/sdb
# vgcreate cinder-volumes /dev/sdb

6.3.3 cinder service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.mysql -u root
2.
   SELECT User, Host, Password FROM mysql.user;


>use cinder;
>show tables;
>delete from services where id=3;

* mysql initailize



6.3.4 dashboard password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http://docs.openstack.org/admin-guide-cloud/content/admin-password-injection.html

systemctl restart httpd.service



6.3.5 floating ip ==>nova
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://www.mirantis.com/blog/configuring-floating-ip-addresses-networking-openstack-public-private-clouds/

nova floating-ip-pool-list

nova-manage floating create --ip_range=  --pool POOL_NAME


vi /etc/nova/nova.conf

public_interface="eth1"

# the pool from which floating IPs are taken by default
default_floating_pool="pub"

6.3.6 firewall
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http://docs.openstack.org/admin-guide-cloud/content/install_neutron-fwaas-agent.html

6.3.7 mariadb delete
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

yum list maria*

yum remove mariadb.x86_64 mariadb-galera-common.x86_64 mariadb-galera-server.x86_64 mariadb-libs.x86_64