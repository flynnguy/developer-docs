Confirm Directory Structure
***************************

1.  Connect to your sphere using :doc:`USB or SSH <enable-ssh>`

2.  The following will create the driver and apps folder in /data/sphere/user-autostart/ and change permissions. It should be safe to run even if the folders already exist. 

::

	sudo mkdir -p /data/sphere/user-autostart/{drivers,apps} && sudo chown -R ninja.ninja /data/sphere

3.  Your Sphere now has the directories where you can load custom applications
