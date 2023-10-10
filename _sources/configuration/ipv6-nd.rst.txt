[ipv6-nd]
=========

Overview ipv6 neighbor discovery section.

**verbose=0|1**
  By default is disabled.

  Specified will ipv6-nd module produce verbose logging.

**MaxRtrAdvInterval=n**
  By default 600 second.

  The maximum time allowed between sending unsolicited multicast router advertisements from the interface, in seconds.

**MinRtrAdvInterval=n**
  By default calculates 0.33 * **MaxRtrAdvInterval**

  The minimum time allowed between sending unsolicited multicast router advertisements from the interface, in seconds.

**AdvManagedFlag=1|0**
  By default disabled.

  When set, hosts use the administered (stateful) protocol for address autoconfiguration in addition to any addresses autoconfigured using stateless address autoconfiguration. The use of this flag is described in RFC 4862.

**AdvOtherConfigFlag=**
  By default disabled.

  When set, hosts use the administered (stateful) protocol for autoconfiguration of other (non-address) information. The use of this flag is described in RFC 4862.

**AdvLinkMTU=**
  By default not defined.

  The MTU option is used in router advertisement messages to insure that all nodes on a link use the same MTU value in those cases where the link MTU is not well known.

  If specified, i.e. not 0, must not be smaller than 1280 and not greater than the maximum MTU allowed for this link (e.g. ethernet has a maximum MTU of 1500. See RFC 4864).

**AdvReachableTime=**
  By default not defined.

  The time, in milliseconds, that a node assumes a neighbor is reachable after having received a reachability confirmation. Used by the Neighbor Unreachability Detection algorithm

**AdvRetransTimer=**
  By default not defined.

  The time, in milliseconds, between retransmitted Neighbor Solicitation messages. Used by address resolution and the Neighbor Unreachability Detection algorithm

**AdvCurHopLimit=**
  By default 64.

  The default value that should be placed in the Hop Count field of the IP header for outgoing (unicast) IP packets. The value should be set to the current diameter of the Internet.
  
**AdvDefaultLifetime=n**
  By default calculating 3 * **MaxRtrAdvInterval**

  The lifetime associated with the default router in units of seconds.

**AdvValidLifetime=**
  By default not defined.

  The length of time in seconds (relative to the time the packet is sent) that the prefix is valid for the purpose of on-link determination.
  
**AdvPreferredLifetime=**
  By default not defined.

  The length of time in seconds (relative to the time the packet is sent) that addresses generated from the prefix via stateless address autoconfiguration remain preferred.

**AdvOnLinkFlag=**

**AdvAutonomousFlag=**

**MaxInitialRtrAdvCount=**

**MaxInitialRtrAdvInterval=**

