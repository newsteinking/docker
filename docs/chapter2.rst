chapter2   docker run
==============================

::

    docker -e GUNICORN_OPTS=[--preload] run --name registry   -p 5000:5000 -v `pwd`/registry/docker-registry-storage:/docker-registry-storage $(USERNAME)/registry

.

2.1 docker usability
--------------------------

2.1.1 crosbymichael/dockerui
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    yum install npm

    make install




https://github.com/crosbymichael/dockerui

Container Quickstart

You must add option  -e GUNICORN_OPTS=[--preload]
::

    docker run -d -p 9000:9000 --privileged -v /var/run/docker1.sock:/var/run/docker1.sock dockerui/dockerui ==>
    docker -e GUNICORN_OPTS=[--preload] run  -p 9000:9000 --privileged -v /var/run/docker.sock:/var/run/docker.sock dockerui/dockerui

.
Open your browser to http://<dockerd host ip>:9000

angularjs

1.install grunt

::

    sudo npm install -g grunt-cli

2. install yoeman
::

        sudo  npm install -g yo


3. install bower
::

    sudo  npm install -g bower

4. install angular generator
::

    sudo npm install -g generator-angular

5. su sean
::

    $ sudo chonw -R 사용자아이디 ~/.npm
    $ su sean
    $ mkdir angularStudy
    $ cd angularStudy
    $ yo angular

    $ grunt server

.
https://github.com/nickholub/angular-dashboard-app

*Running Application


    Node.js way

    Install express

      $ npm install express

    Run Node.js server

      $ node app.js

    Application will be available at http://localhost:3000.

    Simple web server way

    Start any web server in "dist" directory, e.g. with Python

      $ python -m SimpleHTTPServer 8080

    Application will be available at http://localhost:8080
*Running Application (development mode)
Install dependencies:

    $ npm install

stream.js:94
      throw er; // Unhandled stream error in pipe.
            ^
Error: invalid tar file

*install autoconf 2.6.5 by source

./configure --prefix=/usr

make

make check

make install

*install automake 1.14 by source

./configure --prefix=/usr --docdir=/usr/share/doc/automake-1.14.1
make
sed -i "s:./configure:LEXLIB=/usr/lib/libfl.a &:" t/lex-{clean,depend}-cxx.sh
make -j4 check
make install



npm install gulp-imagemin@1.0.1
npm install imagemin@1.0.5
npm install imagemin-gifsicle@1.0.0
npm install gifsicle@1.0.2


Install Bower dependencies:

    $ bower install

Run Grunt server task:

    $ grunt server

Application will be available at http://localhost:9000
*Building Application

pplication is built with Grunt.

    $ npm install -g grunt-cli
    $ grunt






2.1.2 jdeathe/centos-ssh
~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/jdeathe/centos-ssh

manual build

change its value in etc folder ( Docker git directory)

::

    $docker build -rm -t jdeathe/centos-ssh:latest .



Quick Run
::

    docker run -d --name ssh.pool-1.1.1 -p 2020:22  jdeathe/centos-ssh:latest



configuration data volume for shareing

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
    ssh  -p 2020 -i ~/.ssh/id_rsa_insecure root@localhost -o StrictHostKeyChecking=no
    ssh  -p 2020 -i ~/.ssh/id_rsa_insecure app-admin@localhost -o StrictHostKeyChecking=no


OK


.
2.1.3 dockerfiles-centos-ssh
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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

OK


2.1.4 tutum-centos
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


tutum wordpress
https://github.com/tutumcloud/tutum-docker-wordpress.git

.




2.1.5 firefox docker
~~~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/creack/docker-firefox.git

::

    docker build -t sean/ubuntu:12.04 .

    docker run -d -p 5901:5901 <username>/firefox

.

2.1.6 sameersbn/docker-gitlab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/sameersbn/docker-gitlab

Pull the image from the docker index. This is the recommended method of installation as it is easier to update image. These builds are performed by the Docker Trusted Build service.
::

    docker pull sameersbn/gitlab:7.9.0


You can also pull the latest tag which is built from the repository HEAD
::

    docker pull sameersbn/gitlab:latest


Alternately you can build the image locally.

::

    git clone https://github.com/sameersbn/docker-gitlab.git
    cd docker-gitlab
    docker build --tag="$USER/gitlab" .

start

::

    docker run --name='gitlab' -it --rm  -e 'GITLAB_PORT=10080' -e 'GITLAB_SSH_PORT=10022'  -p 10022:22 -p 10080:80  -v /var/run/docker.sock:/run/docker.sock  -v $(which docker):/bin/docker -v /lib64/libdevmapper.so.1.02:/usr/lib/libdevmapper.so.1.02 -v /lib64/libudev.so.0:/usr/lib/libudev.so.0  sameersbn/gitlab:7.9.0

error
libdevmapper.so.1.02: cannot open shared object file....



It's bug, you can fix it, todo the following:
::

    [root@[hostname] bin]# cd /lib64/
    [root@[hostname] lib64]# ln -s /lib64/libdevmapper.so.1.02 /lib64/libdevmapper.so.1.02.1
    [root@[hostname]# ldconfig
    [[root@[hostname]# ldconfig -v |grep libdevmapper.so.1.02.1
    libdevmapper.so.1.02 -> libdevmapper.so.1.02.1


.



.
2.2 Automic run tool
--------------------------


2.2.1 Automic Site
~~~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/projectatomic/atomic-site.git

$ ./ docker.sh&

::

    chcon -Rt svirt_sandbox_file_t source/
    # requires docker and being in the right group
    docker build -t middleman .
    docker run -p 4567:4567 -v "$(pwd)"/source:/tmp/source:ro middleman


and browsing in http://10.3.0.115:4567/ or http://localhost:4567/

2.2.2 Automic image
~~~~~~~~~~~~~~~~~~~~~~~~~

http://www.projectatomic.io/docs/quickstart/

In fedora image , there was continous disconnection when two network was established.
setting
::

    $sudo vi /etc/bashrc

    add NM_CONTROLLED="yes"
    and
    $sudo systemctl stop NetworkManager
    $sudo systemctl disable NetworkManager
    $sudo systemctl restart network


under construction ......



