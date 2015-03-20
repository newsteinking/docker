chapter2   docker run
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


2.1 crosbymichael/dockerui
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

https://github.com/crosbymichael/dockerui




2.1 jdeathe/centos-ssh
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
2.3 dockerfiles-centos-ssh
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

.

2.4 tutum-centos
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
