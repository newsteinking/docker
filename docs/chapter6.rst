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

yum install -y openstack-packstack  openstack-utils

yum install -y screen traceroute bind-utils





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
vi /etc/mongod.conf
#bind_ip = 127.0.0.1
bind_ip = 10.77.241.120

>systemctl restart mongod.service 

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
CONFIG_REPO=http://10.77.241.121/repos/openstack7/rdo,http://10.77.241.121/repos/centos7


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

vi /etc/openstack-dashboard/local_settings

OPENSTACK_HYPERVISOR_FEATURE = {
...
    'can_set_password': False, ==>True
}

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
systemctl restart openstack-nova-compute.service

6.3.6 firewall
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http://docs.openstack.org/admin-guide-cloud/content/install_neutron-fwaas-agent.html

vi /etc/neutron/neutron.conf

service_plugins = firewall
[service_providers]
...
service_provider = FIREWALL:Iptables:neutron.agent.linux.iptables_firewall.OVSHybridIptablesFirewallDriver:default

[fwaas]
driver = neutron_fwaas.services.firewall.drivers.linux.iptables_fwaas.IptablesFwaasDriver
enabled = True

vi /etc/openstack-dashboard/local_settings


'enable_firewall' = True

systemctl restart neutron-l3-agent.service neutron-server.service httpd.service

6.3.7 mariadb delete
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

yum list maria*

yum remove mariadb.x86_64 mariadb-galera-common.x86_64 mariadb-galera-server.x86_64 mariadb-libs.x86_64

6.3.8 juno network setting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://cloudssky.com/en/blog/RDO-OpenStack-Juno-ML2-VXLAN-2-Node-Deployment-On-CentOS-7-With-Packstack/

br-ex port delete
>ovs-vsctl del-port br-ex eth0

#neutron subnet-create osxnet 10.3.4.0/24 --name osx_subnet --dns-nameserver 8.8.8.8
# source keystonerc_osx
# neutron net-create osxnet

# neutron subnet-create osxnet 192.168.32.0/24 --name osx_subnet --dns-nameserver 8.8.8.8
# neutron net-create ext_net --router:external=True

# neutron subnet-create --gateway 10.3.4.100 --disable-dhcp --allocation-pool start=10.3.4.100,end=10.3.4.200 ext_net 10.3.4.0/24 --name ext_subnet

# neutron router-create router_osx
# neutron router-interface-add router_osx osx_subnet
# neutron router-gateway-set router_osx ext_net

* router down
neutron router-port-list router_osx
neutron port-show 6f626532-6deb-4765-9490-349e5ae42f6a


* key stone add

[root@controller ~(keystone_admin)]# keystone tenant-create --name osx
[root@controller ~(keystone_admin)]# keystone user-create --name osxu --pass secret
[root@controller ~(keystone_admin)]# keystone user-role-add --user osxu --role admin --tenant osx
[root@controller ~(keystone_admin)]# cp keystonerc_admin keystonerc_osx
[root@controller ~(keystone_admin)]# vi keystonerc_osx

***
ovs-vsct show






6.3.9 vm network problem
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* open stack vm network problem

host public ip 10.3.4.4  add GATEWAY=10.3.4.1

*ovs-vsctl show

https://cloudssky.com/en/blog/RDO-OpenStack-Juno-ML2-VXLAN-2-Node-Deployment-On-CentOS-7-With-Packstack/

* public network creation
add public network in admin and add DHCP agent
* add /etc/hosts
vi /etc/hosts
10.3.4.4 OpenStackServer2

*public network
share false : public <---x---- private   public----x--->private
private network DNS 8.8.8.8 ==> xxx

*VM instance problem
add same name will error in booting

https://fosskb.wordpress.com/2014/06/10/managing-openstack-internaldataexternal-network-in-one-interface/


6.3.10 Open vSwitch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Perform the following configuration on Host 1:

    Create an OVS bridge:

    ovs-vsctl add-br br0

    Add eth0 to the bridge (by default, all OVS ports are VLAN trunks, so eth0 will pass all VLANs):

    ovs-vsctl add-port br0 eth0

    Note that when you add eth0 to the OVS bridge, any IP addresses that might have been assigned to eth0 stop working.
     IP address assigned to eth0 should be migrated to a different interface before adding eth0 to the OVS bridge.
     This is the reason for the separate management connection via eth1.

    Add VM1 as an "access port" on VLAN 100. This means that traffic coming into OVS from VM1 will be untagged and
    considered part of VLAN 100:

    ovs-vsctl add-port br0 tap0 tag=100

    Add VM2 on VLAN 200.

    ovs-vsctl add-port br0 tap1 tag=200

Repeat these steps on Host 2:

    Setup a bridge with eth0 as a VLAN trunk:

    ovs-vsctl add-br br0 ovs-vsctl add-port br0 eth0

    Add VM3 to VLAN 100:

    ovs-vsctl add-port br0 tap0 tag=100

    Add VM4 to VLAN 200:

    ovs-vsctl add-port br0 tap1 tag=200

6.3.11 openstack-service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

openstack-service start /stop

openstack-status


neutron-db-manage --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugin.ini upgrade head

neutron-db-manage

openstack-db --service neutron --update

openstack-db --service keystone --update


6.3.12 Using VXLAN Tenant Networks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

vi /etc/neutron/plugins/openvswitch/ovs_neutron_plugin.ini
[OVS]
tenant_network_type=vxlan
tunnel_type=vxlan

[AGENT]
tunnel_types=vxlan

6.3.13 OpenvSwitch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Open vSwitch commands:
  init                        initialize database, if not yet initialized
  show                        print overview of database contents
  emer-reset                  reset configuration to clean state

Bridge commands:
  add-br BRIDGE               create a new bridge named BRIDGE
  add-br BRIDGE PARENT VLAN   create new fake BRIDGE in PARENT on VLAN
  del-br BRIDGE               delete BRIDGE and all of its ports
  list-br                     print the names of all the bridges
  br-exists BRIDGE            exit 2 if BRIDGE does not exist
  br-to-vlan BRIDGE           print the VLAN which BRIDGE is on
  br-to-parent BRIDGE         print the parent of BRIDGE
  br-set-external-id BRIDGE KEY VALUE  set KEY on BRIDGE to VALUE
  br-set-external-id BRIDGE KEY  unset KEY on BRIDGE
  br-get-external-id BRIDGE KEY  print value of KEY on BRIDGE
  br-get-external-id BRIDGE  list key-value pairs on BRIDGE

Port commands (a bond is considered to be a single port):
  list-ports BRIDGE           print the names of all the ports on BRIDGE
  add-port BRIDGE PORT        add network device PORT to BRIDGE
  add-bond BRIDGE PORT IFACE...  add bonded port PORT in BRIDGE from IFACES
  del-port [BRIDGE] PORT      delete PORT (which may be bonded) from BRIDGE
  port-to-br PORT             print name of bridge that contains PORT

Interface commands (a bond consists of multiple interfaces):
  list-ifaces BRIDGE          print the names of all interfaces on BRIDGE
  iface-to-br IFACE           print name of bridge that contains IFACE

Controller commands:
  get-controller BRIDGE      print the controllers for BRIDGE
  del-controller BRIDGE      delete the controllers for BRIDGE
  set-controller BRIDGE TARGET...  set the controllers for BRIDGE
  get-fail-mode BRIDGE       print the fail-mode for BRIDGE
  del-fail-mode BRIDGE       delete the fail-mode for BRIDGE
  set-fail-mode BRIDGE MODE  set the fail-mode for BRIDGE to MODE

Manager commands:
  get-manager                print the managers
  del-manager                delete the managers
  set-manager TARGET...      set the list of managers to TARGET...

SSL commands:
  get-ssl                     print the SSL configuration
  del-ssl                     delete the SSL configuration
  set-ssl PRIV-KEY CERT CA-CERT  set the SSL configuration

Switch commands:
  emer-reset                  reset switch to known good state

Database commands:
  list TBL [REC]              list RECord (or all records) in TBL
  find TBL CONDITION...       list records satisfying CONDITION in TBL
  get TBL REC COL[:KEY]       print values of COLumns in RECord in TBL
  set TBL REC COL[:KEY]=VALUE set COLumn values in RECord in TBL
  add TBL REC COL [KEY=]VALUE add (KEY=)VALUE to COLumn in RECord in TBL
  remove TBL REC COL [KEY=]VALUE  remove (KEY=)VALUE from COLumn
  clear TBL REC COL           clear values from COLumn in RECord in TBL
  create TBL COL[:KEY]=VALUE  create and initialize new record
  destroy TBL REC             delete RECord from TBL
  wait-until TBL REC [COL[:KEY]=VALUE]  wait until condition is true
Potentially unsafe database commands require --force option.

Options:
  --db=DATABASE               connect to DATABASE
                              (default: unix:/var/run/openvswitch/db.sock)
  --no-wait                   do not wait for ovs-vswitchd to reconfigure
  --retry                     keep trying to connect to server forever
  -t, --timeout=SECS          wait at most SECS seconds for ovs-vswitchd
  --dry-run                   do not commit changes to database
  --oneline                   print exactly one line of output per command

Logging options:
  -vSPEC, --verbose=SPEC   set logging levels
  -v, --verbose            set maximum verbosity level
  --log-file[=FILE]        enable logging to specified FILE
                           (default: /var/log/openvswitch/ovs-vsctl.log)
  --syslog-target=HOST:PORT  also send syslog msgs to HOST:PORT via UDP
  --no-syslog             equivalent to --verbose=vsctl:syslog:warn

Active database connection methods:
  tcp:IP:PORT             PORT at remote IP
  ssl:IP:PORT             SSL PORT at remote IP
  unix:FILE               Unix domain socket named FILE
Passive database connection methods:
  ptcp:PORT[:IP]          listen to TCP PORT on IP
  pssl:PORT[:IP]          listen for SSL on PORT on IP
  punix:FILE              listen on Unix domain socket FILE
PKI configuration (required to use SSL):
  -p, --private-key=FILE  file with private key
  -c, --certificate=FILE  file with certificate for private key
  -C, --ca-cert=FILE      file with peer CA certificate

Other options:
  -h, --help                  display this help message
  -V, --version               display version information
6.3.14 OpenvSwitch  in Allinone
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
All in one with ens8

ovs-vsctl add-br br-ens8

ovs-vsctl add-port br-ens8 ens8

ifconfig br-ens8 10.3.4.4 up


ip link set br-ens8 promisc on

ip link add proxy-br-eth1 type veth peer name eth1-br-proxy

ip link add proxy-br-ex type veth peer name ex-br-proxy

ovs-vsctl add-br br-eth1

ovs-vsctl add-br br-ex

ovs-vsctl add-port br-eth1 eth1-br-proxy

ovs-vsctl add-port br-ex ex-br-proxy

ovs-vsctl add-port br-ens8 proxy-br-eth1

ovs-vsctl add-port br-ens8 proxy-br-ex


ip link set eth1-br-proxy up promisc on

ip link set ex-br-proxy up promisc on

ip link set proxy-br-eth1 up promisc on

ip link set proxy-br-ex up promisc on

*router ping

ip netns

ip netns exec qdhcp-9cbd5dd0-928a-4808-ae34-4cc2563fa619 ip addr


route add -net 192.168.32.0/24 gw 10.3.4.100

10.3.4.4->10.3.4.100->192.168.32.1  Ok

6.4 OpenStack Juno +OpenDaylight Helium
-------------------------------------------

https://www.rdoproject.org/Helium_OpenDaylight_Juno_OpenStack
https://wiki.opendaylight.org/view/OVSDB:Helium_and_Openstack_on_Fedora20#VMs


