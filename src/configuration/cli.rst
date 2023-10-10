.. _cli_configuration:

[cli]
=====

Configuration overview of the command line interface.

**verbose=1|2**
  By default ``verbose=1``

  If ``verbose=1`` then cli module will log IP address of each connection. 
  
  If ``verbose=2`` then cli module will also log passed commands.

**tcp=host:port**
  By default is not defined.
  
  Defines on which IP address and port the TCP module will listen for incoming connections. When host is empty, the TCP module listens on all local interfaces. It isn't loaded if this option isn't defined.

**telnet=host:port**
  By default is not defined.

  Defines on which IP address and port the Telnet module will listen for incoming connections. When host is empty, the Telnet module listens on all local interfaces. It isn't loaded if this option isn't defined.

**password=passwd**
  By default is not defined.

  Defines the password to be used by the TCP and Telnet modules for authenticating clients. No authentication is performed if this option isn't defined.
  
**prompt=prompt**
  By default ``prompt=accel-ppp``

  Defines the prompt string used by the Telnet module.

**history-file=filename**
  By default ``history-file=/var/lib/accel-ppp/history``

  Defines the file used by the Telnet module for loading and storing its command history.

**sessions-columns=column_list**
  By default ``sessions-columns=ifname,username,calling-sid,ip,rate-limit,type,comp,state,uptime``

  Defines the default set of columns to be displayed by the ``show sessions`` command. Invalid column names are silently discarded. All possible params:
  
  * ``ifname`` - interface name
  * ``username`` - username
  * ``calling-sid`` - calling station identifier, for *PPPoE* and *IPoE start=dhcpv4* is client mac-address, for *PPTP*, *L2TP*, *SSTP* and *IPoE start=up* is client ip addres.
  * ``called-sid`` - called station identifier,  for *PPPoE* and *IPoE start=dhcpv4* is server mac-address, for *PPTP*, *L2TP*, *SSTP* and *IPoE start=up* is server ip addres.
  * ``sid`` - session identifier
  * ``ip``  - client ip address
  * ``ip6`` - client ipv6 prefix
  * ``ip6-dp`` - delegated ipv6 prefix for client
  * ``rate-limit`` - rate-limit, required param ``[modules]shaper``, otherwise this column not displayed.
  * ``type`` - session type, may contain next connection types: *ipoe*, *pppoe*, *pptp*, *l2tp*, *sstp*
  * ``comp`` - compression/ecnryption method
  * ``state`` - state of session, may contain next states: *start*, *active*, *finish*
  * ``uptime`` - human readable session uptime 
  * ``uptime-raw`` - session uptime in seconds
  * ``rx-bytes`` - human readable received bytes
  * ``tx-bytes`` - human readable transmitted bytes
  * ``rx-bytes-raw`` - received bytes
  * ``tx-bytes-raw`` - transmitted bytes
  * ``rx-pkts`` - received packets
  * ``tx-pkts`` - transmitted packets
  * ``netns`` - network namespace name
  * ``vrf`` - Virtual Routing and Forwarding 
  * ``ipoe-type`` - IPoE session type (UP/DHCP)
