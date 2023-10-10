FAQ
===============

HTB: quantum of class is big
----------------------------
When an error appears in the logs:

.. code-block:: sh

  HTB: quantum of class 10001 is big. Consider r2q change.

You need to set a parameter for the shaper:

.. code-block:: sh

  #accel-pp will calculate the value itself
  moderate-quantum=1


How to rotate logs ?
--------------------

You can use system logrotate utility for it. Put following file to /etc/logrotate.d

.. code-block:: sh

  /var/log/accel-ppp/*.log {
        missingok
        sharedscripts
        postrotate
                test -r /var/run/accel-pppd.pid && kill -HUP `cat /var/run/accel-pppd.pid`
        endscript
  }

I don't see pppd processes, how to manually terminate session ?
---------------------------------------------------------------

Yes, in fact accel-ppp doesn't use pppd because it has its own ppp implementation.
To terminate session you may use three methods:

1. Use cli (telnet or tcp):

.. code-block:: sh

  By default telnet listens connections on 2000 port and tcp on 2001 port.
  $ telnet 127.0.0.1 2000
  Trying 127.0.0.1...
  Connected to 127.0.0.1.
  Escape character is '^]'.
  accel-ppp version 1.5.0
  accel-ppp# terminate if ppp0


or

.. code-block:: sh

  $ echo 'terminate if ppp0' | nc -q0 127.0.0.1 2001


There are also other criterias to select session(s), use help cli command to get more information.

2. Use radius Disconnect-Message:

.. code-block:: sh

  $ echo 'NAS-Port=0' | radclient 127.0.0.1:3799 disconnect testing123
  Received response ID 170, code 41, length = 20

and you can control it in logs:

.. code-block:: sh

  [2012-01-21|16:48:55]:  info: ppp0: recv [RADIUS|Disconnect-Request id=aa <NAS-Port 0>]
  [2012-01-21|16:48:55]:  info: ppp0: send [RADIUS|Disconnect-ACK id=aa]

3. Use snmp:

.. code-block:: sh

  $ snmpset -m +ACCEL-PPP-MIB -v 2c -c local 127.0.0.1 ACCEL-PPP-MIB::termByIfName.0 = ppp0


