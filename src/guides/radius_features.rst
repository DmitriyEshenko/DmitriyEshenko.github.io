RADIUS and DM/CoA features
==========================

Since from commit version 385c403 accel-ppp support VRF (Virtual Routing and Forwarding).

Usually, this feature is useful to isolate clients e.g. put client interface to some context with different routing and firewall rules.
User interface can be put to VRF context via RADIUS Access-Accept packet, or change it via RADIUS CoA.

Accel-ppp uses own RADIUS vendor dictionary https://github.com/accel-ppp/accel-ppp/blob/master/accel-pppd/radius/dict/dictionary.accel and RADIUS attribute ``Accel-VRF-Name``

All VRFs should be manually created in advance:

.. code-block:: sh

  ip link add VRF_NAME type vrf table RT_TABLE_ID
  ip link set dev VRF_NAME up

Linux VRF documentation https://www.kernel.org/doc/Documentation/networking/vrf.txt 

If ``Accel-VRF-Name`` is used in Access-Accept message, but VRF was not created then the session will not be established.

Set VRF via CoA
---------------

Put user interface to some VRF context

.. code-block:: sh

  echo 'User-Name=bob, Accel-VRF-Name="red"' | radclient -x 127.0.0.1:3799 coa testing123

Delete user interface from VRF context

.. code-block:: sh

  echo 'User-Name=bob, Accel-VRF-Name="0"' | radclient -x 127.0.0.1:3799 coa testing123
  
If ``Accel-VRF-Name`` is used in CoA message and VRF does not exist then CoA-NAK will be sent.

