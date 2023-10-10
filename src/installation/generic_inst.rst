Generic Installation
--------------------

Requirment
^^^^^^^^^^

* modern linux distribution
* kernel-2.6.25 or later
* cmake-2.6 or later
* libnl-2.0 or probably later (optional, required for builtin shaper)
* libcrypto-0.9.8 or probably later (openssl-0.9.8)
* libpcre
* net-snmp-5.x (optional, required for snmp)
* libssl-0.9.8 or probably later (openssl-0.9.8)
* liblua5.1 probably later (optional, required for create username fundamental on packet header information)

Compilation and instalation
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Make sure you have configured kernel headers in /usr/src/linux, or specify other location via KDIR.

Download accel-ppp source code with git client, tree master . Master tree contain actual patches last release.

.. code-block:: sh

  git clone https://github.com/accel-ppp/accel-ppp.git accel-ppp-code

Create directory for build source code and go to this directory. 

.. code-block:: sh

  mkdir /opt/accel-ppp-code/build
  cd /opt/accel-ppp-code/build/


.. code-block:: sh

  cmake [-DBUILD_DRIVER=FALSE] [-DKDIR=/usr/src/linux] [-DCMAKE_INSTALL_PREFIX=/usr/local] [-DCMAKE_BUILD_TYPE=Release] [-DLOG_PGSQL=FALSE] [-DSHAPER=FALSE] [-DRADIUS=TRUE] [-DNETSNMP=FALSE] ..

.. admonition:: Notice:

  Please note that the double dot record in the end of the command is essential.
  You'll probably get error or misconfigured sources if you miss it.

BUILD_DRIVER, KDIR, CMAKE_INSTALL_PREFIX, CMAKE_BUILD_TYPE, LOG_PGSQL, SHAPER, RADIUS are optional,
but while pptp is not present in mainline kernel you probably need BUILD_DRIVER.

.. code-block:: sh

  make
  make install
  
Run
^^^

.. code-block:: sh

  accel-pppd -d -p /var/run/accel-ppp.pid -c /etc/accel-ppp.conf

.. code-block:: sh

  usage: accel-pppd [-d] [-p <file>] -c <file>
  	where:
  		-d - daemon mode
  		-p - write pid to <file>
  		-c - config file

Control
^^^^^^^
Accel-ppp support next features for control daemon and sessions

* **accel-cmd**
* **telnet**
* **radius CoA**
* **snmp**
