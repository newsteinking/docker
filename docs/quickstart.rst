1. Quickstart
==============

1.1 리눅스
---------------

자동 설치 스크립트

Docker는 리눅스 배포판 종류를 자동으로 인식하여 패키지를 설치해주는 스크립트를 제공합니다.
::

    $ sudo wget -qO- https://get.docker.com/ | sh

get.docker.com 스크립트로 Docker를 설치하면 hello-world 이미지도 자동으로 설치됩니다.

hello-world 이미지는 사용하지 않을 것이므로 모두 삭제합니다.
::

    $ sudo docker rm `sudo docker ps -aq`
    $ sudo docker rmi hello-world
.


우분투
~~~~~~~~~~

자동 설치 스크립트를 사용하지 않고 우분투에서 패키지로 직접 설치하는 방법입니다.

우분투 14.04 LTS 64비트를 기준으로 하겠습니다.
::

    $ sudo apt-get update
    $ sudo apt-get install docker.io
    $ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker


/usr/bin/docker.io 실행파일을 /usr/local/bin/docker로 링크해서 사용합니다.
RedHat Enterprise Linux, CentOS

자동 설치 스크립트를 사용하지 않고, 레드햇 엔터프라이즈 리눅스(RHEL)와 CentOS에서 패키지로 직접 설치하는 방법입니다.
 RHEL과 CentOS 패키지 저장소에는 docker-io가 없으므로 EPEL(Fedora Extra Packages For Enterprise Linux) 저장소를
 사용할 수 있도록 설정합니다.

CentOS 6
~~~~~~~~~~~

::$ sudo yum install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    $ sudo yum install docker-io

AWS EC2에 설치되는 Amazon Linux(CentOS 기반)는 EPEL 저장소를 바로 사용할 수 있으므로 epel-release-6-8.noarch.rpm은 설치하지 않아도 됩니다.

CentOS 7에서는 docker 패키지를 설치하면 됩니다.

CentOS 7
~~~~~~~~~
::

    $ sudo yum install docker

Docker 서비스 실행
::

    $ sudo service docker start

부팅했을 때 자동으로 실행하기
::

    $ sudo chkconfig docker on

1.2 Mac OS X
-----------------
맥에서는 Boot2Docker를 이용하여 Docker를 사용할 수 있습니다.

https://github.com/boot2docker/osx-installer/releases13에서 Boot2Docker-1.x.x.pkg를 받은 뒤 설치합니다.

설치는 특별한 것이 없으므로 따로 설명하지 않겠습니다(내부적으로 VirtualBox가 함께 설치됩니다).

설치가 끝난 뒤에 응용 프로그램(Applications)에서 boot2docker를 실행합니다.

잠시 기다리면 자동으로 boot2docker.iso를 이용하여 가상 머신이 생성되고, 가상 머신에 접속됩니다.


1.3  윈도우
-----------
윈도우에서는 Boot2Docker를 이용하여 Docker를 사용할 수 있습니다.

https://github.com/boot2docker/windows-installer/releases52에서 docker-install.exe를 받은 뒤 설치합니다.