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




6.2 route
------------------------








