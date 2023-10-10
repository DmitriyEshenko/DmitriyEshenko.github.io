[pppoe]
=======

Configuration of PPPoE module.

**verbose=0|1**
  By default is disabled.

  Specified will pppoe module produce verbose logging.

**padi-limit=n**
  By default is unlimited ``padi-limit=0``

  Specifies overall limit of PADI packets to reply in 1 second period. Rate of per-mac PADI packets is limited to no more than 1 packet per second. May also used as per-interface param.

**interface=[re:]ifname[,padi-limit=n]**
  By default is defined. Important to set this option.
  
  Specifies interface name to listen/send discovery packets. May be specify multiple interface options. If *ifname* is prefixed with ``re:`` then *ifname* is considered as regular expression. Optional padi-limit parameter specifies limit of PADI packets to reply on this interface in 1 second period.

**ac-name=ac-name**
  By default is ``ac-name=accel-ppp`` 

  ``Need fix: ìmportant check it`` Specifies AC-Name tag value. If absent tag will not be sent.

**service-name=service-name1,service-nameN**
  By default is not defined.

  Specifies Service-Name to respond. If absent any Service-Name is acceptable and client's Service-Name will be sent back.
  Also possible set multiple service-names: ``service-name=sn1,sn2,sn3``

**accept-any-service=n**
  By default is disabled.

  If service-name specified still will answer with service names, but accepts any service name in PADR request. Useful for scenarios, where selection of PPPoE done by client, based on service names in PADO.

**pado-delay=delay[,delay1:count1[,delay2:count2[,...]]]**
   By default is disabled.
   
   Specifies delays (also in condition of connection count) to send PADO (ms). Last delay in list may be -1 which means don't accept new connections. List have to be sorted by count key.
   
**called-sid=ifname|mac|ifname:mac**
  By default is ``called-sid=mac``

  Specifies how to represent Called-Station-ID.
  
* ``ifname`` - Called-Station-ID will contain name of interface accepted request.
* ``mac`` - Called-Station-ID will contain mac address of interface accepted request.
* ``ifname:mac`` - Called-Station-Id will contain both name and mac of interface.

**tr101=0|1**
  By default is enabled ``tr101=1``

  Specifies whether to handle TR101 tags.

**mppe=deny|allow|prefer|require**
   By default is not defined.
   
   Default behavior - don’t ask client for mppe, but allow it if client wants.

**ifname=ifname**
  By default is not defined.

  If this option is given ppp interface will be renamed using ifname as a template, i.e ``ifname=pppoe%d`` => *pppoe0*.

.. admonition:: Note:
    
  Also interface may renamed if RADIUS server send attribute ``NAS-Port-Id`` with custom name. Length this value not be more 16 characters.

**ifname-in-sid=called-sid|calling-sid|both**
  By default is not defined.

  Specifies that interface name should be present in Called-Station-ID or in Calling-Station-ID or in both attributes.

**sid-uppercase=0|1**
  By default is lowercase ``sid-uppercase=0```

  Specifies in which case sen attribute ``Called-Station-ID`` and ``Calling-Station-ID``.
  
  Example: lowercase ``Calling-Station-Id "xx:xx:xx:xx:xx:xx"``, uppercase ``Calling-Station-Id "XX:XX:XX:XX:XX:XX"``

**cookie-timeout=n**
  By default ``cookie-timeout=5``

  

**ip-pool=pool_name**
  By default is not defined.

  Specifies ip pool name which accel-ppp will use for allocate client ip address.
  
.. admonition:: Note:
    
    For use ippool need add this module to ``[modules]`` section, and sets params on section ``[ip-pool]``

**ipv6-pool=pool_name**
  By default is not defined.

  Specifies ipv6 pool name which accel-ppp will use for allocate client ipv6 prefix.

**ipv6-pool-delegate=pool_name**
  By default is not defined.

  Specifies ipv6 prefix delegation pool name which accel-ppp will use for allocate client ipv6 prefix delegation.

**vlan-mon=[re:]name[,filter]**
  vlan-mon needs for automatically crate vlans interfaces. Support regular expression (re:). Parameter specifies list of vlans or ranges of vlans to monitor for and may be in following form: ``vlan-mon=eth1,2,5,10,20-30``

**vlan-timeout=n**
  By default: ``vlan-timeout=60``.
  
  Specifies time on second of vlan inactivity before it will be removed.

**vlan-name=pattern**
  By default vlan-name=%I.%N
  
  Specifies pattern of vlan interface name. Pattern may contain following macros:

* ``%I`` - name of pattern interface.

* ``%N`` - number of vlan.

* ``%P`` - number of vlan of parent interface.

