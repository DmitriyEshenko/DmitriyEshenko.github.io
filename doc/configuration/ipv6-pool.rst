[ipv6-pool]
===========

Overview ipv6 pool section.

**vendor=vendor**
  By default not defined.
  
  If attribute is vendor-specific then specify vendor name in this option.

**attr-prefix=attribute**
  By default ``attr-prefix=Delegated-IPv6-Prefix-Pool``

  Specifies which Radius attribute contains delegated prefix pool name.

**attr-address=attribute**
  By default ``attr-address=Stateful-IPv6-Address-Pool``
  
  Specifies which Radius attribute contains stateful address pool name.

**ipv6prefix/mask,prefix_len[,name=pool_name][,next=next_pool_name]**
  By default not defined.

  fc00:0:1::/48,64 - specifies pool of address by dividing prefix fc00:0:1::/48 to networks with 64 prefix len, e.g:
  
.. code-block:: sh

  fc00:0:1:0::/64
  fc00:0:1:1::/64
  ...
  fc00:0:1:ffff::/64

**delegate=ipv6prefix/mask,prefix_len[,name=pool_name][,next=next_pool_name]**
  By default not defined.

  Specifies range of prefixes to delegate to clients through DHCPv6 prefix delegation (rfc3633).  Format is same as described above.

**gw-ip6-address=ipv6address**
  By default not defined.
  
  Specifies gateway address (used only for /128 prefixes)
