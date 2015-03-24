.. _`LinuxCMD`:

chapter 3 :Linux Command
============================

3.1 Basic
------------------------

3.1.1 Directory Size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  display directory size

::

    $ du -hs  [directory name]


3.1.2 manual core dump
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    $echo c > /proc/sysrq-trigger   or ALT+SysRq+C

core dump make in following

/var/crash/xxx/vmcore


3.1.3 grub
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

change kernel booting sequence

::

    $vi /boot/grub/grub.conf



3.1.4 crash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    sys -
    bt -
    ps - Process list
    free - Memory
    mount -
    irq - .
    kmem -
    log -
    mod -
    net -
    runq -
    task -
    rd -
    foreach -
    set -
    struct -
    files -


.



3.2 Package Install
--------------------------------

3.2.1  kernel debug info
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

kernel debugging infor

::

    $yum --enablerepo=debug install kernel-debuginfo-'uname -r'


/usr/lib/debug/lib/modules/'uname -r'/vmlinux


3.2.2  ELREPO  add
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

kernel debugging info install


To install ELRepo for RHEL-7, SL-7 or CentOS-7:
::

    $rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm (external link)

To make use of our mirror system, please also install yum-plugin-fastestmirror.

To install ELRepo for RHEL-6, SL-6 or CentOS-6:

::

    rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm (external link)

To make use of our mirror system, please also install yum-plugin-fastestmirror.

To install ELRepo for RHEL-5, SL-5 or CentOS-5:

::

    rpm -Uvh http://www.elrepo.org/elrepo-release-5-5.el5.elrepo.noarch.rpm (external link)



3.2.3  CentOS Desktop & X windows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



::

    $yum -groupinstall "Desktop" "Desktop Platform" "X window system" "Fonts"


3.2.4  CentOS Development
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

CentOS basic development install

::

    $yum install gcc
    $yum groupinstall "Development Tools"
    $yum install ncurses-devel
    $yum install libncurses5-dev
    $yum install python-dev

.



3.2.5  HTTP Tunneling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

this is not good

install package
::

    yum install httptunnel


On Server side
::

    $hts -F <server_ip_addr>:<port_of_your_app> 80
    $hts -F 10.3.0.115:80 80
    $hts -F 10.77.241.121:80 80


On Client side
::

    $htc -P <my_proxy.com:proxy_port> -F <port_of_your_app> <server_ip_addr>:80
    $htc -P 10.3.0.115:80 -F 80 10.3.0.115:80
    $htc -P 10.77.241.121:80 -F 80 10.77.241.121:80

.
3.2.6  Linux Route add
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

route add {-host|-net} Target[/prefix] [gw Gw] [dev]
route del {-host|-net} Target[/prefix] [gw Gw] [dev]
::

    [root@localhost ~]# route  add  -net  192.168.200.0/24  gw  192.168.100.1  dev  bond0
    [root@localhost ~]# route  add  -host  192.168.200.100  gw  192.168.100.1  dev  bond1

or
::

    route add -net 10.77.212.0/24 gw  10.77.241.1 dev eth1

delete
::

    route del -net 10.77.212.0/24

.
3.2.7  user list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Task: Linux List Users Command

To list only usernames type the following awk command:
::

    $ awk -F':' '{ print $1}' /etc/passwd

 .





3.3 CentOS7,RHEL7,Fedora 21
--------------------------------

3.3.1  service start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Stop service:
::

    systemctl stop httpd


Start service:
::

    systemctl start httpd



Restart service (stops/starts):
::

    systemctl restart httpd



Reload service (reloads config file):
::

    systemctl reload httpd




List status of service:
::

    systemctl status httpd



What about chkconfig? That changed too? Yes, now you want to use systemctl for the chkconfig commands also..

chkconfig service on:
::

    systemctl enable httpd


chkconfig service off:
::

    systemctl disable httpd


chkconfig service (is it set up to start?)
::

    systemctl is-enabled httpd


chkconfig –list (shows what is and isn’t enabled)
::

    systemctl list-unit-files --type=service


.




3.3.2  add servcie
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

OS used in this guide: CentOS 7 with EPEL for the iperf3 package

1. First, install iperf3.
::

    $ sudo yum install iperf3

.

2. Next, create a user iperf which will be used to run the iperf3 service.
::

    $ sudo adduser iperf -s /sbin/nologin

.

3. Next, create the following file:
::


    /etc/systemd/system/iperf3.service

.


Put in the following contents and save the file:
::

    [Unit]
    Description=iperf3 Service
    After=network.target

    [Service]
    Type=simple
    User=iperf
    ExecStart=/usr/bin/iperf3 -s
    Restart=on-abort


    [Install]
    WantedBy=multi-user.target

.


Done.
Start the iperf3 service:
::

    $ sudo systemctl start iperf3


Check the status:

[stmiller@ny ~]$ sudo systemctl status iperf3
iperf3.service - iperf3 Service
   Loaded: loaded (/etc/systemd/system/iperf3.service; disabled)
   Active: active (running) since Mon 2014-12-08 13:43:49 EST; 18s ago
 Main PID: 32657 (iperf3)
   CGroup: /system.slice/iperf3.service
           └─32657 /usr/bin/iperf3 -s

Dec 08 13:43:49 ny.stmiller.org systemd[1]: Started iperf3 Service.
[stmiller@ny ~]$

Stop the iperf3 service:
::

    $ sudo systemctl stop iperf3


Start the service at boot:

[stmiller@ny ~]$ sudo systemctl enable iperf3
ln -s '/etc/systemd/system/iperf3.service' '/etc/systemd/system/multi-user.target.wants/iperf3.service'

Disable the service at boot:
::

    $ sudo systemctl disable iperf3

.


3.3.3  user list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.