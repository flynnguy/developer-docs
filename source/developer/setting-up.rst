Setting up a Local Development Environment
******************************************

This will outline how to setup your development environment for Linux or Mac OS. 

Required Software
=================

You will need to have the following installed: (See platform specific notes below)

* `go 1.4.2 <http://golang.org/doc/go1.4>`_
* `git <http://git-scm.com/>`_, `mercurial <http://mercurial.selenic.com/>`_
* `docker <https://www.docker.com/>`_
* `Virtual Box <https://www.virtualbox.org/wiki/Downloads>`_ (Mac only)
* `boot2docker <https://docs.docker.com/installation/mac/>`_ (Mac only)

Test your environment
=====================

You can test your environment by running the following commands and you should get a similar output:
::

    $ git version
    git version 1.9.5 (Apple Git-50.3)

    $ hg version
    Mercurial Distributed SCM (version 3.3.2)
    (see http://mercurial.selenic.com for more information)

    Copyright (C) 2005-2014 Matt Mackall and others
    This is free software; see the source for copying conditions. There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

    $ go version
    go version go1.4.2 darwin/amd64

the following steps are not required for Linux hosts
++++++++++++++++++++++++++++++++++++++++++++++++++++
::

    $ VBoxManage --version
    4.3.26r98988

    $ boot2docker version 
    Boot2Docker-cli version: v1.5.0

    $ boot2docker status
    running

    $ docker version
    Client version: 1.5.0
    Client API version: 1.17
    Go version (client): go1.4.1
    Git commit (client): a8a31ef
    OS/Arch (client): darwin/amd64
    Server version: 1.5.0
    Server API version: 1.17
    Go version (server): go1.4.1
    Git commit (server): a8a31ef

Ninja Sphere Development CLI
============================

Now you will need to download the Ninja Sphere Development CLI with the following command:
::

    go get github.com/ninjasphere/ninja-dev-cli/ninja

Once this is done, you should be able to run the ninja command
::

    $ ninja
    Usage:
      ninja [options] build <path>
      ninja -h | --help

Compile a Driver
----------------

For this example, we are going to build the [Chrome Cast driver](https://github.com/ninjasphere/driver-go-chromecast) but any driver should work the same way.

First, use go get to download the code and all it's dependencies
::

    go get -d github.com/ninjasphere/driver-go-chromecast

You should now have a driver-go-chromecast folder with the source code for the driver in it.
::

    $ cd $GOPATH/src/github.com/ninjasphere/driver-go-chromecast
    $ ls -1
    LICENSE
    Makefile
    README.md
    device.go
    driver.go
    main.go
    ninjapack
    package.json
    release.go
    scripts
    version.go

Run the build
-------------

Using docker and the Ninja Sphere Development CLI makes building a driver or app package simple:
::

    $ ninja build .

This should produce the following files:
::

    $ ls -1 *.{snap,deb}
    driver-go-chromecast_0.1.0_amd64.snap
    driver-go-chromecast_0.1.0_armhf.deb
    driver-go-chromecast_0.1.0_armhf.snap

The .dev file can be installed (using dpkg -i) on a Ninja Sphere kickstarter edition that runs Ubuntu 14.04. The _armhf.snap file can be installed (with snappy install) on a Ninja Sphere developer edition that runs Snappy Ubuntu Core. If you get read only issues when trying to install your package, you may need to use 'sudo with-rw'
::

    $ sudo with-rw dpkg -i driver-go-chromecast_0.1.0_armhf.deb


Platform Specific Notes
=======================

Linux
+++++

Go
--

To install the latest version of go, you can use `GVM <https://github.com/moovweb/gvm>`_.
::

    $ sudo apt-get install curl git mercurial make binutils bison gcc build-essential
    $ bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
    $ source ~/.gvm/scripts/gvm
    $ gvm install go1.4.2
    $ gvm use go1.4.2 --default

If you don't use GVM, make sure you set your GOPATH and GOROOT as well as updating your PATH.
::
    mkdir -p ~/sphere/go;
    echo -e '
    export GOROOT=/usr/local/go;
    export GOPATH=~/sphere/go;
    export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
    ' >> ~/.bashrc
    . ~/.bashrc

Docker
------
**Do not use the docker.io apt package from the Ubuntu 14.04 distribution as that version is quite old and is not compatible with the Ninja Sphere Development CLI.**

If you are not using a recent version of ubuntu, refer to the `Linux installation instructions <https://docs.docker.com/installation/>`_ on docker's website. If you are, the following should work.
::

    $ wget -qO- https://get.docker.com/ | sh
    $ sudo usermod -a -G docker $(whoami)

You will need to logout and then back in.

OS X
++++
Download and install `Virtual Box 4.3.26 for OSX hosts <http://download.virtualbox.org/virtualbox/4.3.26/VirtualBox-4.3.26-98988-OSX.dmg>`_ if you do not already have it. Ensure that you select the options that enable the Virtual Box command line tools.

You should also install homebrew (warning, if you use macports this might break things)
::

    # Install homebrew if you haven't already:
    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    # If you have already installed it, you might want to update it
    $ brew update
    # Install go and mercurial, alternatively use gvm for go and just install mercurial
    $ brew install go mercurial
    # If you don't use Docker's preferred installation method
    $ brew install docker boot2docker

If you don't use GVM, be sure to set your GOPATH and GOROOT
::

    mkdir -p ~/sphere/go;
    echo -e 'export GOPATH=~/sphere/go;
    export PATH=$GOPATH/bin:$PATH' >> ~/.bash_profile
    source ~/.bash_profile

If docker version reports a mismatch between the client and server versions, run:
::

    boot2docker stop
    boot2docker download
    boot2docker start
    docker version

If when running 'docker version' you get a TLS error, you may need to do the following:
::

    $(boot2docker shellinit)

Windows
+++++++

Currently Windows is not supported as a Ninja Sphere development environment in part because the Windows build of boot2docker doesn’t ship a native docker CLI for Windows.

One path for Windows users interested in doing development for the Ninja Sphere might be to install Ubuntu inside either Virtual Box or VMWare virtual machine and then follow the Linux instructions within that VM.
