Recommendations
===============

Enable forwarding
-----------------
To enable packet forwarding need edit /etc/sysctl.conf and add or uncomment next:

.. code-block:: sh

  net.ipv4.ip_forward=1
  net.ipv6.conf.all.forwarding=1
  
For apply this params now, use command ``sysctl -p`` or after reboot server this params will be applied automatically.

MTU
---

If used vlan-per-user often required 802.1ad standard also called as QinQ or Q-in-Q, then need to set MTU on main interface and S-VLAN, because adding to headed one more field.
Interface which using QinQ usually consist of ``<interface_name>.<S-VLAN>.<C-VLAN>``.
S-VLAN (Service VLAN) is TAG which wrap C-VLAN (Customer VLAN).

As example: 

.. code-block:: sh

  MTU
             1504
               |   1504
               |   |   1500
               |   |   |
            eth0.2001.101
               |   |   |
               |   |   C-VLAN
               |   S-VLAN
               Interface
   
Set up MTU on interface eth0 and interface with S-VLAN

.. code-block:: sh

  ip link set eth0 mtu 1504
  ip link set eth0.2001 mtu 1504

.. admonition:: Note:

  If used ``bonding`` need change MTU on *bonding* (bond0) and *slaves* (eth0, eth1 ...) interfaces.

Increase ARP cache size
-----------------------------

If accel-ppp used as DHCP BRAS important to increase ARP cache size, otherwise you can cache overflow and clients have lost connections. Edit /etc/sysctl.conf and add next:

.. code-block:: sh

  net.ipv4.neigh.default.gc_thresh1 = 4096
  net.ipv4.neigh.default.gc_thresh2 = 8192
  net.ipv4.neigh.default.gc_thresh3 = 12288
  net.ipv6.neigh.default.gc_thresh1 = 4096
  net.ipv6.neigh.default.gc_thresh2 = 8192
  net.ipv6.neigh.default.gc_thresh3 = 12288

For apply this params now, use command ``sysctl -p`` or after reboot server this params will be applied automatically.
