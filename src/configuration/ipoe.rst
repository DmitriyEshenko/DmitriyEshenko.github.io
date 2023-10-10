.. _ipoe:

[ipoe]
------
Method authentication users, control sessions and delivery without any tunnel "called" as IPoE (IP over Ethernet).
Accel-ppp support L2 and L3 topologies and start sessions on DHCP Discover or unclassified packet.

Develop auxiliary kernel module for sessions start on unclassified packet and shared interfaces.
This module creates virtual interface, an analogue of ifb and used for sessions shaper and One-to-one NAT.

The difference between L2 and L3. L2 incoming packet will be checked for the mac address set at the session start, and outgoing packets will be sent straight to this mac address without additional ARP requests, which provides protection against IP/mac address spoofing.
In the case of L3, the outgoing packet will be routed according to the established routing rules.

IPoE configuration overview
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Section IPoE contain many flexible customization.

**[ipoe]**

**verbose=0|1**
    Default value is ``verbose=0``

    Writes more detailed logs.

**ipv6=0|1**
    By default is disabled: ``ipv6=0``

    Activate support ipv6 globally. Also may defined per-interface. Required modules ``ipv6_nd``, ``ipv6_dhcp`` and  ``ipv6pool`` if ipv6 addresses will allocate accel-ppp.

**mode=L2|L3**
    By default mode is L2.
    
    Parameter specifies client connectivity mode. ``mode=L2`` then it means that clients are on same network where interfaces. ``mode=L3`` means that client are behind some router.  Also may defined per-interface.

**start=dhcpv4|up|auto**
    By default is not defined. Important to set this.
    
    Parameter specifies which way session starts:
    
    * **dhcpv4** - start on DHCP Discover.

    * **up** - unclassified packet.

    * **auto** - means automatically start session with username=interface name. Use it with conjunction vlan_mon.

    Also may defined per-interface.

**lua-file=/path/to/file.lua**
     By default is not defined.
     
     Needs only if used lua functions for create username from packet header information. Often used with DHCP Option 82. Look :ref:`lua_examples` for more information.

**username=ifname|lua:function**
    By default for DHCP sessions ``username=ifname``, for sessions start by unclassified packet (``start=up``) ``username`` is client ip address.

    If ``username=ifname`` then interface name from which packet was arrived will be used as username.


    If ``username=lua:username`` then lua function with name ``username`` will be called to construct username from dhcp packet fields.
    Also may defined per-interface.

**password=username|csid|empty|<string>**
    By default ``password=username``
    Specifies how to generate password.
    
    If ``password=username`` then password will be same as username.

    If ``password=csid`` then password will be same as Calling-Station-Id.
    
    Also you can specify fixed password in ``<string>`` or leave empty.

**session-timeout=n**
     By default is disabled: ``session-timeout=0``

    Define max sessions time in seconds. After this time session will be terminated. May redefine with radius attribute **Session-Timeout**

**idle-timeout=n**
    By default is disabled ``idle-timeout=0`` 
    
    Specifies timeout in seconds to wait for any packets from client, after this time session will terminated if client don't send any packet. Often used with ``mode=L3``.

**lease-time=n**
    By default ``lease-time=600``

    Specifies lease time in seconds to be sent to DHCP client.

**max-lease-time=n**
    By default ``max-lease-time=660``

    Specifies max lease time in seconds, after this time session will be terminated if client won't renew it.

**renew-time=n**
    By default ``renew-time`` calculate as lease-time/2.

    Specifies lease renew time (option 58) in seconds to be sent to DHCP client. Might be overwritten by RADIUS attribute DHCP-Renewal-Time.

**rebind-time=n**
    By default ``rebind-time`` calculate as lease-time/2+lease-time/4+lease-time/8.

    Specifies lease rebind time (option 59) in seconds to be sent to DHCP client. Might be overwritten by RADIUS attribute DHCP-Rebinding-Time.

**shared=0|1**
    By default is active ``shared=1``
    
    Specifies where interface is shared by multiple users. If used vlan-per-user need turn this to 0. Also may defined per-interface.
    
**unit-cache=n**
    By default is disabled: ``unit-cache=0``

    Specifies number of interfaces to keep in cache. It means that don't destory interface after corresponding session is destoyed, instead place it to cache and use it later for new sessions repeatedly. Actial only if used shared interfaces.

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
    vlan-mon needs for automatically crate vlans interfaces, more often on vlan-per-user schemas. Support regular expression (**re:**). Parameter specifies list of vlans or ranges of vlans to monitor for and may be in following form: vlan-mon=eth1,2,5,10,20-30
    
**vlan-timeout=n**
    By default: ``vlan-timeout=60``.
    Specifies time on second of vlan inactivity before it will be removed.
    
**vlan-name=pattern**
    By default ``vlan-name=%I.%N``
    
    The vlan-name parameter allows you to specify the pattern for the VLAN interface name.

    The pattern may include the following macros:

    ``%I``: Represents the name of the parent interface (e.g. ethX, enoX, enpXsY, etc.).
    
    ``%N``: Represents the number of the VLAN (the latest tag ID). In the case of Q-in-Q, this refers to the C-VLAN.
    
    ``%P``: Represents the number of the VLAN for the parent interface. In the case of Q-in-Q, this refers to the S-VLAN.
    
    For example, if the parent interface name is eth0 and the VLAN number is 10, the VLAN interface name would be eth0.10 based on the default pattern %I.%N.
        
    Works with interface params and required regular expression.
  
**noauth=0|1**
    By default is disabled: ``noauth=0`` and used RADIUS or chap-secrets authentication.

    Allows users to connect without authentication by radius or chap-secrets. For correct work it is necessary to use with ip-pool.

**ifcfg=0|1**
    By default is active: ``ifcfg=1``

    Parameter specifies whether accel-ppp should add router IP address and route to client to interface or it is explicitly configured. Also may defined per-interface.

**proto=n**
    By default 3 - boot.
    
    Specifies number of protocol to be used for inserted routes. Works only with ``ifcfg=0``, when the routes create an accel-ppp, not a kernel. Also need exist gw ip address in the system on any of the interfaces, otherwise an error will be output to the accel-ppp.log

.. admonition:: Log output:

    debug: libnetlink: RTNETLINK answers: Invalid argument

**check-mac-change=0|1**
    By default is active: ``check-mac-change=1``
    
    Terminate session when detects change of mac address of client.

**soft-terminate=0|1**
    By default is disabled: ``soft-terminate=0``

    When terminating sessions through ``cli`` or ``Radius Disconnect-Message``, the session will not be terminated immediately, but will be marked as finished and client will continue working, but next time renew lease the session will be terminated. Session will terminate immediately when expired `max-lease-time`. For manually terminate session immediately you may use cli command ``accel-cmd terminate <session selector> hard``

.. code-block:: sh

    accel-cmd terminate if ipoe0 hard
    
**l4-redirect-table=n**
     By default is disabled: ``l4-redirect-table=0``
     
     Specifies number of table. If L4-Redirect radius attribute is received and it's value is not 0 or '0' then accel-ppp will add following rule: ip rule add from <client_ip> table

**l4-redirect-ipset=<name>**
    By default is not defined.
     
     Specifies name of ipset list. If L4-Redirect radius attribute is received and it's value is not 0 or '0' then accel-ppp will add client's ip to that ipset name.

**l4-redirect-on-reject=n**
    By default is disabled: ``l4-redirect-on-reject=0``

    Specified time in seconds for creating temporary sessions if radius rejects access and  'ip rule add from ip_addr table l4-redirect-table' rule will be created.

**l4-redirect-ip-pool=pool_name**
    By default is not defined.

    Allocates ip address from specified pool name if radius rejects access. Pool must be sets in section `[ip-pool]`

**agent-remote-id=<identifier>**
    By default is not defined.

    If accel-ppp used as DHCP relay, than to DHCP requests will inserted Option 82 with agent-remote-id and agent-circuit-id with interface name from which received client request.

**local-net=x.x.x.x/mask**
    By default is not defined.
    
    Specifies networks from which packets will be treated as unclassified. Need only for ``start=up``. You may specify multiple local-net options. For example:

.. code-block:: sh

    local-net=100.64.0.0/24
    local-net=192.168.0.0/24
    local-net=172.16.0.0/24

**vendor=<vendor name>**
    By default is not defined.
    
    Specifies vendor name for RADIUS attributes in current section. For using RADIUS DHCP attributes, set ``vendor=dhcp``

**attr-dhcp-client-ip=<attribute>**
    By default is not defined.

    Specified radius attribute which contains ip address for assign to client. Example with existing attribute:
    
.. code-block:: sh

    attr-dhcp-client-ip=DHCP-Client-IP-Address

.. admonition:: Note:

    If set custom attribute then need add its for both (radius server and accel-ppp) dictionaries.
    
**attr-dhcp-router-ip=<attribute>**
    By default is not defined.

    Specified radius attribute which contains router ip address for assign to client. Example with existing attribute:
    
.. code-block:: sh

    attr-dhcp-router-ip=DHCP-Gateway-IP-Address
    
.. admonition:: Note:

    If set custom attribute then need add its for both (radius server and accel-ppp) dictionaries.

**attr-dhcp-mask=<attribute>**
    By default is not defined.

    Specified radius attribute which contains netmask (CIDR) for assign to client. Example with existing attribute:

.. code-block:: sh

    attr-dhcp-mask=DHCP-Subnet-Mask

.. admonition:: Note:

    If set custom attribute then need add its for both (radius server and accel-ppp) dictionaries.

**attr-dhcp-lease-time=<attribute>**
    By default is not defined.

    Specified radius attribute which contains lease time in seconds to be sent to DHCP client. This attribute has priority and may redefine value which sets in ``lease-time`` sets globally.

**attr-dhcp-renew-time=<attribute>**
    By default is not defined.
    
    Specified radius attribute which contains lease renew time (option 58) in seconds to be sent to DHCP client. This attribute has priority and may redefine value which sets in ``renew-time`` sets globally.

**gw-ip-address=x.x.x.x/mask**
    By default is not defined.
    
    Specifies address to be used as server ip address if radius can assign only client address. In such case if client address is matched network and mask then specified address and mask will be used. You can specify multiple such options.
    For example:

.. code-block:: sh

    gw-ip-address=100.64.0.1/24
    gw-ip-address=192.168.0.1/24
    gw-ip-address=172.16.0.0/24

**attr-dhcp-opt82=<attribute>**
    By default is not defined.

    Specifies radius attribute which will contain option 82 from DHCP packet header in binary and send to radius server.
    Example:

.. code-block:: sh

    attr-dhcp-opt82=DHCP-Option82
    
.. admonition:: Note:

    Need add custom attribute in both radius and accel-ppp dictionaries. By default dictionary is located at ``/usr/share/accel-ppp/radius/dictionary`` if accel-ppp build as pkg DEB or RPM. Dictionary path may be redefine in section ``[radius]``.

    Example adding custom attribute:

.. code-block:: sh

    ATTRIBUTE       DHCP-Option82             245 octets
    

**attr-dhcp-opt82-remote-id=<attribute>**
    By default is not defined.

    Specifies radius attribute which will contain only **Agent Remote Id** from DHCP packet header and send to radius server. Example with existing attribute in dictionary:

.. code-block:: sh

    attr-dhcp-opt82-remote-id=DHCP-Agent-Remote-Id

**attr-dhcp-opt82-circuit-id=<attribute>**
    By default is not defined.
    
    Specifies radius attribute which will contain only **Agent Circuit Id** from DHCP packet header and send to radius server. Example with existing attribute in dictionary:

.. code-block:: sh

    attr-dhcp-opt82-circuit-id=DHCP-Agent-Circuit-Id
    
**offer-timeout=n**   
    By default ``offer-timeout=10``
    
    Specified time in seconds which accel-ppp wait DHCP request  from client. If client don't send DHCP request for this time, accel-ppp terminate session.
    
**offer-delay=delay[,delay1:count1[,delay2:count2[,...]]]**
    By default is not defined.
    
    One of load balancing mechanism. specifies delays in milliseconds (also in condition of connection count) to send DHCPOFFER . Last delay in list may be -1 which means don't accept new connections. List must to be sorted by count key. Example:

.. code-block:: sh

     offer-delay=0,100:1000,200:2500,300:5000,400:9999,-1:10000

.. admonition:: Explain:

    Clients from 1 to 999 take DHCP offers without delay, client from 1000 to 2499 take DHCP offers with delay 100 ms, clients from 2500 to 4999 take DHCP offers with delay 200 ms, clients from 5000 to 9999 take DHCP offers with delay 300 ms, last client take DHCP offer with delay 400 ms and accel-ppp no more accept connections.
    
**weight=n**
    By default not defined:
 
    More modern load balancing mechanism based on weight.
    
    How it works:
    On reception of DHCPDISCOVER accel-ppp sends broadcast DHCP message to port 67 with same xid and add special vendor-specific option where encodes its current session count multiplied by weight. On reception of such message accel-ppp searches session with same xid and compares weight. If received weight is less than session's weight then it terminates this session.
    May be used as per-interface.

.. admonition:: Note:

    Per-interface weight=0 has special meaning as backup (fail-over) interface, f.e. it terminates session on any received weight.

**calling-sid=mac|ip**
    By default ``calling-sid=mac``

    Specifies value of Calling-Station-Id radius attribute.

**proxy-arp=0|1|2**
    By default is disabled: ``proxy-arp=0``

    Parameter specifies whether accel-ppp should reply to arp requests. Also may defined per-interface.
    
    ``0`` -  proxy-arp disabled.

    ``1`` -  proxy-arp enabled. Accel send arp-reply if src ip and dst ip on different interfaces (as well as linux proxy_arp).

    ``2`` -  proxy-arp enabled. Accel send arp-reply back to the same interface (as well as linux proxy_arp_pvlan).

.. admonition:: Note:
    
  Works only for subnets defined in `local-net` param
    
**ip-unnumbered=0|1**
    By default is enabled: ``ip-unnumbered=1``

    Specifies should accel-ppp create route for session with netmask /32. May be used as per-interface.

**interface=[re:]name**
    By default interface has many params which explain below.
    
    Specifies interface to listen dhcp or unclassified packets. If name is prefixed with **re:** then name is treated as **regular expression**.
    
    May be specify multiple interface options, for example:

.. code-block:: sh

    interface=eth0,mode=L3,start=UP,shared=1
    interface=re:^eth1\.[0-9]+\.[0-9][0-9][0-9]$,mode=L2,shared=0,start=dhcpv4,mtu=1500,ifcfg=1

The ``mode=L2|L3`` parameter specifies client connectivity mode. If ``mode=L2`` then it means that clients are on same network where interface is. ``mode=L3`` means that client are behind some router.
    
The ``shared=0|1`` parameter specifies where interface is shared by multiple users or it is vlan-per-user.
    
The ``start=dhcpv4|up|auto`` parameter specifies which way session starts.

    * ``dhcpv4`` - start by DHCP Discover packet.
    
    * ``up`` -  start by unclassified packet.
    
    * ``auto`` - means automatically start session with ``username=interface`` name. Use it with conjunction vlan_mon.

The ``ipv6``

The ``mtu=n`` parameter specifies whether accel-ppp should change MTU(maximum transmission unit) on interfaces. By default not set and MTU value inherited from root interface. Often used for vlan-per-user (QinQ).

The ``range=x.x.x.x/mask`` parameter specifies local range of ip address to give to dhcp clients. First IP in range is router IP. If you need more customization use ``ip-pool`` instead of ``range``.
    
The ``ifcfg=0|1`` parameter specifies whether accel-ppp should add router IP address and route to client to interface or it is explicitly configured. By default inheris global ``ifcfg`` value.
    
The ``relay=x.x.x.x`` parameter specifies DHCPv4 relay IP address to pass requests to. If specified giaddr is also needed.

The ``giaddr=x.x.x.x`` parameter specifies relay agent IP address.

The ``src=x.x.x.x`` parameter specifies ip address to use as source when adding route to client.

The ``username=ifname|lua:function_name`` allow set custom LUA function to form username from packet header information. Often used this param on varius BRAS connection type.

``ipv6=0|1`` will activate support ipv6 on interface. If not defined, inherit global params.

``weight=n`` is load balancing mechanism based on weight. ``weight=0`` has special meaning as backup (fail-over) interface, f.e. it terminates session on any received weight.
