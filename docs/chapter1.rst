chapter 1: Docker
==================


1.1 Basic
-------------------

1.1.1 Linux
~~~~~~~~~~~~~~~~

Automatic Install Script


::

    $ sudo wget -qO- https://get.docker.com/ | sh

remove hell-world

::

    $ sudo docker rm `sudo docker ps -aq`
    $ sudo docker rmi hello-world


.


Ubuntu


Manual install for Ubuntu4.04

::

    $ sudo apt-get update
    $ sudo apt-get install docker.io
    $ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker



RedHat Enterprise Linux, CentOS



CentOS 6

::

    $ sudo yum install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    $ sudo yum install docker-io



CentOS 7


::

    $ sudo yum install docker

Docker service execution

::

    $ sudo service docker start

auto execution during boot

::

    $ sudo chkconfig docker on

1.1.2 Mac OS X
~~~~~~~~~~~~~~~~~~~~~~~



https://github.com/boot2docker/osx-installer/releases13
Boot2Docker-1.x.x.pkg



1.1.3  windows
~~~~~~~~~~~~~~~~~~~~


https://github.com/boot2docker/windows-installer/releases52

docker-install.exe

1.2 Installation
------------------------------

1.2.1 docker default directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.

* docker default directory change


In CentOS 6.5


::

    service docker stop
    mkdir /data/docker  (new directory)
    vi /etc/sysconfig/docker

add following line

::

    other_args=" -g /data/docker -p /var/run/docker.pid"

then save the file and start docker again

::

    service docker start


and will make repository file in /data/docker

1.2.2 Kernel Upgrade 2.6->3.8
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


::

    yum install http://www.elrepo.org/elrepo-release-6-5.el6.elrepo.noarch.rpm
    yum --enablerepo=elrepo-kernel install kernel-ml


.when remote access

   cannot access if kernel is not upgrade


1.2.3 docker start error
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


::

    usr/bin/docker: relocation error: /usr/bin/docker: symbol dm_task_get_info_with_deferred_remove,
    version Base not defined in file libdevmapper.so.1.02 with link time reference

.

::

    yum-config-manager --enable public_ol6_latest

    yum install device-mapper-event-libs


.


1.2.4  Build your own image from CentOS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



::

    yum install feboostrap
    febootstrap -i iputils -i vim-minimal -i iproute -i bash -i coreutils -i
    yum centos centos http://centos.mirror.iweb.ca/6.4/os/x86_64/ -u http://centos.mirror.iweb.ca/6.4/updates/x86_64/


and
::

    [root@banshee ~]# cd centos/
    [root@banshee centos]# tar -c . | docker import - centos


or ISO mount
::

    # mkdir rootfs
    # mount -o loop /path/to/iso rootfs
    # tar -C rootfs -c . | docker import - rich/mybase

using osirrox
::

    yum install xorriso
    osirrox -indev blahblah.iso -extract / /tmp/blahblah
    tar -C /tmp/blahblah -cf- . | docker import blahblah


* save docker images to tar

::

    docker save ubuntu > /tmp/ubuntu.tar



extract ubuntu.tar and jump to lagest directory and will see layer.tar




1.2.5 docker images delete
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*(none) image delete
::

    $ docker rmi $(docker images -f dangling=true | awk '{ print $3 }' | grep -v IMAGE)

*all container delete
::

    $ sudo docker rm $(docker ps -a -q)

*all image delete

::

    $ sudo docker rmi -f $(docker images -q)

.



1.2.6  gunicorn error
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    yum erase python-pip
    yum install xz-libs

# Let's download the installation file using wget:
::

    wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz

# Extract the files from the archive:
::

    tar -xvf setuptools-1.4.2.tar.gz

# Enter the extracted directory:
::

    cd setuptools-1.4.2

.

Install setuptools using the Python we've installed (2.7.6)

::

    python2.7 setup.py install

source install

::

    wget https://pypi.python.org/packages/source/p/pip/pip-1.2.1.tar.gz

    @annmoon-linux ~]# tar xvfz pip-1.2.1.tar.gz
    [root@annmoon-linux ~]# cd pip-1.2.1
    [root@annmoon-linux ~]# python setup.py install



.

*install gunicorn

::

    pip install gunicorn

.

1.2.7  make a private registry
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ref  :https://blog.codecentric.de/en/2014/02/docker-registry-run-private-docker-image-repository/

https://github.com/lukaspustina/docker-registry-demo

::

    $git clone https://github.com/lukaspustina/docker-registry-demo


    make base
    make registry
    make start-registry


.

* error
W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/trusty/InRelease

vi /etc/default/docker

::

    DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"

.

* docker remote error

::

    FATA[0002] Error: Invalid registry endpoint https://10.3.0.115:5000/v1/: Get https://10.3.0.115:5000/v1/_ping: EOF.
    If this private registry supports only HTTP or HTTPS with an unknown CA certificate,
    please add `--insecure-registry 10.3.0.115:5000` to the daemon's arguments. In the case of HTTPS,
    if you have access to the registry's CA certificate, no need for the flag; simply place the CA
    certificate at /etc/docker/certs.d/10.3.0.115:5000/ca.crt

.

in all access server, will insert --insecuur-registry

other_args=" -g /data/docker -p /var/run/docker.pid --insecure-registry 10.3.0.115:5000 "

Edit the config file "/etc/default/docker"

    sudo vi /etc/default/docker

add the line at the end of file

    DOCKER_OPTS="$DOCKER_OPTS --insecure-registry=192.168.2.170:5000"

(replace the 192.168.2.170 with your own ip address)

and restart docker service

    sudo service docker restart



*make registry error

/docker-registry-demo/registry/docker-registry
::

    python setup.py install

docker-registry-demo/registry/docker-registry/requirements
pip install -r main.txt


SWIG/_m2crypto.i:30: Error: Unable to find 'openssl/opensslv.h'
::

    yum install openssl-devel

.


* proxy error
 requirements.insert(0, 'argparse==1.2.1')

/docker-registry-demo/registry/Dockerfile
/docker-registry-demo/registry/docker-registry/Dockerfile

proxy setting

/Dockerfile

::

    ENV http_proxy 'http://10.3.0.172:8080'
    ENV https_proxy 'http://10.3.0.172:8080'
    ENV HTTP_PROXY 'http://10.3.0.172:8080'
    ENV HTTPS_PROXY 'http://10.3.0.172:8080'
    RUN export http_proxy=$HTTP_PROXY
    RUN export https_proxy=$HTTPS_PROXY


.


* pip error

::

    File "/usr/lib/python2.7/dist-packages/requests/utils.py", line 636, in except_on_missing_scheme
    raise MissingSchema('Proxy URLs must have explicit schemes.')
    MissingSchema: Proxy URLs must have explicit schemes.

.

* pin reinstall

::

    [root@annmoon-linux ~]# wget https://pypi.python.org/packages/source/p/pip/pip-1.2.1.tar.gz
    [root@annmoon-linux ~]# tar xvfz pip-1.2.1.tar.gz
    [root@annmoon-linux ~]# cd pip-1.2.1
    [root@annmoon-linux ~]# python setup.py install


    pip install --proxy http://user:password@proxyserver:port TwitterApi

    pip install --proxy="user:password@server:port" packagename


    python setup.py install

.



* local repository push
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now the new feature! To push to or pull from your own registry, you just need to add the
registry’s location to the repository name. It will look like my.registry.address:port/repositoryname


Let’s say I want to push the repository “ubuntu” to my local registry,
which runs on my local machine, on the port 5000:

docker push localhost.localdomain:5000/ubuntu

It’s important to note that we’re using a domain containing a “.” here, i.e. localhost.domain.
Docker looks for either a “.” (domain separator) or “:” (port separator) to learn that the first
part of the repository name is a location and not a user name. If you just had localhost
without either .localdomain or :5000 (either one would do) then Docker would believe that localhost is a username,
as in localhost/ubuntu or samalba/hipache. It would then try to push to the default Central Registry.
Having a dot or colon in the first part tells Docker that this name contains a hostname
and that it should push to your specified location instead.


docker example
~~~~~~~~~~~~~~~~~~~~~~
[REGISTRY]/[IMAGE_NAME]
::

    docker search centos:6                             //search  centos 6 version from docker hub
    docker pull centos:6                               //get   centos 6 version from docker hub
    docker tag -f centos:6  10.3.0.115:5000/centos6    //tag centos 6 version with local ip/port
    docker push 10.3.0.115:5000/centos6                // push centos 6 in local repository

in other machine
::

    docker pull 103.0.115:5000/centos6

.


*redhat registry
::

    docker search registry.access.redhat.com/rhel
    docker pull registry.access.redhat.com/rhel6.5


* remote search

[REGISTRY]/[IMAGE_NAME]

::

    docker search [my.registry.host]:[port]/library  //xxx
    docker search 10.3.0.115:5000/library             //xxx
    curl http://10.3.0.115:5000/v1/repositories/hello_world/tags/latest //000

    curl -X GET http://10.3.0.115:5000/v1/search   // XXX
    curl -X GET http://10.3.0.115:5000/v1/search?q=registry //XXX


.
* change default directory
vi /etc/sysconfig/docker
comment out -g /data/docker

and will be saved local image in /var/lib/docker



*RUN
::

    docker run -p 8080:8080 atcol/docker-registry-ui

.

*RUN Registry UI
::
    docker search atcol/docker-registry-ui
    docker pull atcol/docker-registry-ui
    docker run -p 8080:8080 -e REG1=http://registry_host.name:5000/v1/ atcol/docker-registry-ui
    docker run -p 8080:8080 -e REG1=http://10.3.0.115:5000/vi atcol/docker-registry-ui


and browsing to localhost:8080/.
.
*docker https

Docker version > 1.3.1 communicates over HTTPS by default when connecting to docker registry


* docker search http proxy setting

vi /etc/sysconfig/docker
insert following


##sean
::

    export HTTP_PROXY=http://10.3.0.172:8080
    export HTTPS_PROXY=http://10.3.0.172:8080

* dockerfile http proxy

::

    ENV http_proxy 'http://user:password@proxy-host:proxy-port'
    ENV https_proxy 'http://user:password@proxy-host:proxy-port'
    ENV HTTP_PROXY 'http://user:password@proxy-host:proxy-port'
    ENV HTTPS_PROXY 'http://user:password@proxy-host:proxy-port'

.

sample
::

    ENV http_proxy 'http://10.3.0.172:8080'
    ENV https_proxy 'http://10.3.0.172:8080'
    ENV HTTP_PROXY 'http://10.3.0.172:8080'
    ENV HTTPS_PROXY 'http://10.3.0.172:8080'

.






* netstat
netstat -tulpn

*Dockerfile from local images

You can use it without doing anything special. If you have a local image called blah you can do FROM blah.
If you do FROM blah in your Dockerfile, but don't have a local image called blah,
then Docker will try to pull it from the registry.

In other words, if a Dockerfile does FROM ubuntu, but you have a local image called
ubuntu different from the official one, your image will override it.



1.2.8  Basic certification
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

/etc/hosts

127.0.0.1       localhost
127.0.1.1       ubuntu
<Registry Server IP Address>    registry.example.com


openssl genrsa -out server.key 2048

openssl req -new -key server.key -out server.csr


openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

$ sudo cp server.crt /etc/pki/ca-trust/source/anchors/
$ sudo update-ca-trust enable
$ sudo update-ca-trust extract


in client, copy server.crt and execute 3


yum install httpd-tools



1.2.9  Dockerfile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ref :https://github.com/CentOS/CentOS-Dockerfiles.git
::

    git clone https://github.com/CentOS/CentOS-Dockerfiles.git

    docker build --rm=true -t my/image .




1.2.10  docker run
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


* jdeathe/centos-ssh
~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/jdeathe/centos-ssh
Quick Run
::

    docker run -d --name ssh.pool-1.1.1 -p 2020:22  jdeathe/centos-ssh:latest



configuration data volume
::

    mkdir -p /etc/services-config/ssh.pool-1

    docker run --name volume-config.ssh.pool-1.1.1  -v /etc/services-config/ssh.pool-1:/etc/services-config/ssh busybox:latest /bin/true

    $docker stop ssh.pool-1.1.1
    $docker rm ssh.pool-1.1.1
    $docker run -d  --name ssh.pool-1.1.1 -p :22 --volumes-from volume-config.ssh.pool-1.1.1 jdeathe/centos-ssh:latest



Now you can find out the app-admin, (sudoer), user's password by inspecting the container's logs

::

    $ docker logs ssh.pool-1.1.1   //docker logs <docker container name>

.
Connect to the running container using SSH

If you have not already got one, create the .ssh directory in your home directory with the permissions required by SSH.

::

    $ mkdir -pm 700 ~/.ssh

Get the Vagrant insecure public key using curl (you could also use wget if you have that installed).

::

    $ curl -LsSO https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant
    $mv vagrant ~/.ssh/id_rsa_insecure
    $ chmod 600 ~/.ssh/id_rsa_insecure

If the command ran successfully you should now have a new private SSH key installed in your home "~/.ssh"
directory called "id_rsa_insecure"


Next, unless we specified one, we need to determine what port to connect to on the docker host.
You can do this with ether docker ps or docker inspect. In the following example we use docker ps to
show the list of running containers and pipe to grep to filter out the host port.

::

    $ docker ps | grep ssh.pool-1.1.1 | grep -oe ':[0-9]*->22\/tcp' | grep -oe ':[0-9]*' |cut -c 2-

To connect to the running container use:

::

    ssh -p <container-port> -i ~/.ssh/id_rsa_insecure app-admin@<docker-host-ip>  -o StrictHostKeyChecking=no
    ssh  -p 49154 -i ~/.ssh/id_rsa_insecure app-admin@10.3.0.115  -o StrictHostKeyChecking=no
    ssh  -p 49154 -i ~/.ssh/id_rsa_insecure app-admin@localhost  -o StrictHostKeyChecking=no

is not good ....

.
* dockerfiles-centos-ssh
~~~~~~~~~~~~~~~~~~~~~~~~~

https://github.com/CentOS/CentOS-Dockerfiles/tree/master/ssh/centos6

Building & Running

Copy the sources to your docker host and build the container:

::

    # docker build -rm -t <username>/ssh:centos6 .
    # docker build -rm -t sean/ssh:centos6 .

To run:
::

    # docker run -d -p 22 sean/ssh:centos6


To test, use the port that was just located:

::

    # ssh -p xxxx user@localhost
    # ssh -p 49155 user@localhost

.

* tutum-centos
~~~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/tutumcloud/tutum-centos

To create the image tutum/centos with one tag per CentOS release, execute the following commands on the tutum-ubuntu repository folder:

::

    docker build -t tutum/centos:latest .

    docker build -t tutum/centos:centos5 centos5

    docker build -t tutum/centos:centos6 centos6

    docker build -t tutum/centos:centos7 centos7

Run a container from the image you created earlier binding it to port 2222 in all interfaces:
::

    sudo docker run -d -p 0.0.0.0:2222:22 tutum/centos

The first time that you run your container, a random password will be generated for user root. To get the password, check the logs of the container by running:

::

    docker logs <CONTAINER_ID>

If you want to use a preset password instead of a random generated one, you can set the environment
variable ROOT_PASS to your specific password when running the container:
::

    docker run -d -p 0.0.0.0:2222:22 -e ROOT_PASS="mypass" tutum/centos
    docker run -d -p 0.0.0.0:2222:22 -e ROOT_PASS="1234" tutum/centos


.
