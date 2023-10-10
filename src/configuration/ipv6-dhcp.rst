[ipv6-dhcp]
===========

Overview ipv6 DHCP section.

**verbose=0|1**
  By default is disabled.

  Specified will ipv6-dhcp module produce verbose logging.

**pref-lifetime=n**
  By default ``pref-lifetime=604800``

  Specifies preferred liftime in seconds to be sent to DHCPv6 client.

**valid-lifetime=n**
  By default ``valid-liftime=2592000``

  Specifies valid lifetime in seconds to be sent to DHCPv6 client.

**route-via-gw=0|1**
  By default ``route-via-gw=1``

  Specifies if delegated prefix routes should use gateway.

**server-id=a:b:c:d**
  By default ``server-id=0:0:0:1``

  Specifies the DHCPv6 server-id. Each value a, b, c, d is hex value in range 0000 to ffff
