[dns]
=====

Overview DNS section. 

**dns1=x.x.x.x**
  By default is not defined.

  Specifies primary DNS to be sent to peer.

**dns2=x.x.x.x**
  By default is not defined.

  Specifies secondary DNS to be sent to peer.

.. admonition:: Note:

    Params in this section also may be applied with ``accel-cmd reload`` command, but for new connections.

Also *accel-ppp* has very interesting way to allocate DNS addresses which sent RADIUS server. Received RADIUS attributes is more prior than params in config. For *ppp* (pppoe, pptp, l2tp, sstp) connection type used attributes ``MS-Primary-DNS-Server``, ``MS-Secondary-DNS-Server``. For ipoe connection type need send attributes ``DHCP-Domain-Name-Server``

.. code-block:: sh
  
  +----+-------------------+-------------------------+----+-------------------------+
  | id | username          | attribute               | op | value                   |
  +----+-------------------+-------------------------+----+-------------------------+
  |  1 | user              | DHCP-Domain-Name-Server | := | 100.64.254.254          |
  |  2 | user              | DHCP-Domain-Name-Server | := | 192.168.254.254         |
  
  +----+-------------------+-------------------------+----+-------------------------+
  | id | username          | attribute               | op | value                   |
  +----+-------------------+-------------------------+----+-------------------------+
  |  3 | user              | MS-Primary-DNS-Server   | := | 100.64.254.254          |
  |  4 | user              | MS-Secondary-DNS-Server | := | 192.168.254.254         |
