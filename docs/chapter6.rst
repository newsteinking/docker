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




6.2 packstack install
------------------------

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
vi /etc/monogodb.conf
#bind_ip = 127.0.0.1
bind_ip = 10.77.241.120






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




