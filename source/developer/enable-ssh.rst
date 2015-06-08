Enable SSH
**********

1. Connect to your sphere using USB
===================================

*OS X*::

	$ screen /dev/tty.usbmodem*

*Linux*::

	$ screen /dev/ttyACM0

*Windows*::

	- use PuTTY and select the "Serial" option
	- connect to the COM port number labelled "ELMO GMAS" in Windows Device Manager e.g. COM7
	- use Speed 115200
	- Press OPEN
	- Press ENTER when black terminal screen appears

**If you don't want to enable ssh, you can stop here. You are now connected via USB.**

2.  Login with default username and password
============================================
::

	username: ninja
	password: temppwd

3.  Change the default ninja password
=====================================

Before enabling ssh, it is good practice to change the default password of the ninja account to something less well-known, so:
::

    $ sudo with-rw passwd ninja
    # type the current password once and the new password twice

Don't forget the new password - if you do forget it, you will need to factory reset the sphere to reset it.

4.  Install ssh for easier access
=================================
::

	$ sudo with-rw bash -c "apt-get update -y && apt-get install -y openssh-server"

5.  ssh from your Terminal program
==================================
::

	$ ssh ninja@ninjasphere.local
