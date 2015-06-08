MQTT
****

Mosquitto
=========

The ninjasphere runs `mosquitto <http://mosquitto.org/>`_ for MQTT locally. You can read more about MQTT with `MQTT 101 - How to Get Started with the lightweight IoT Protocol <http://www.hivemq.com/how-to-get-started-with-mqtt/>`_.

To see what's going on, you can run the following from your sphere:
::
    $ mosquitto_sub -v -t '#'

Alternatively, if you have mosquitto installed on your computer, you can specify the ninjasphere with the -h parameter:
::
    $ mosquitto_sub -h ninjasphere.local -t '#'


Most messages to/from devices and the models etc. are defined in the schemas:  https://github.com/ninjasphere/schemas
