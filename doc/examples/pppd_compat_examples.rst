.. _pppd_compat_examples:

pppd-compat examples
====================

Accel-ppp module ``[pppd-compat]`` is useful to execute scripts when **ip-up|ip-down|ip-change** event for customer's session occurs.

Examples below show how to put cusomer's IPv4 & IPv6 to specific ipsets, depending on the value of received RADIUS-attribute named ``Filter-Id``. For example, it can be useful if one needs to grant access from **customer ipset** only to **specific ipset**.

Example Accel-ppp configuration:

.. code-block:: sh

  [modules]
    pppd_compat

  [pppd-compat]
    ip-up=/etc/accel-ppp_ip-up.sh
    ip-down=/etc/accel-ppp_ip-down.sh
    ip-change=/etc/accel-ppp_ip-up.sh
    radattr-prefix=/run/radattr

.. admonition:: Note:

    **ipsets** must exist before scripts are executed.

Example ipsets creation:

.. code-block:: sh

  #!/bin/sh

  ipset create soc_res_v4 hash:net family inet
  ipset create soc_res_v6 hash:net family inet6
  ipset create blk_res_v4 hash:net family inet
  ipset create blk_res_v6 hash:net family inet6
  ipset create blk_usr_v4 hash:ip family inet
  ipset create soc_usr_v6 hash:net family inet6
  ipset create soc_usr_v4 hash:ip family inet
  ipset create blk_usr_v6 hash:net family inet6

Example /etc/accel-ppp_ip-up.sh script:

.. code-block:: sh

  #!/bin/sh

  # Option "Active".
  ACTIVE_FILTER_ID=1

  # Option "Paysystems".
  BLOCK_SET_V4='blk_usr_v4'
  BLOCK_SET_V6='blk_usr_v6'
  BLOCK_FILTER_ID=2

  # Option "Social".
  SOCIAL_SET_V4='soc_usr_v4'
  SOCIAL_SET_V6='soc_usr_v6'
  SOCIAL_FILTER_ID=3

  # argv[5], contains IPv4-address,
  # (https://github.com/xebd/accel-ppp/blob/master/accel-pppd/extra/pppd_compat.c).
  IPV4=$5

  # argv[1], contains interface name.
  RADATTR='/run/radattr.'$1

  # Add|delete client's IPv4|IPv6 addresses to a specific ipset.
  # $IPV6_PREFIX and $IPV6_DELEGATED_PREFIX are environment variables of Accel-ppp,
  # (https://github.com/xebd/accel-ppp/blob/master/accel-pppd/extra/pppd_compat.c).
  if [ -f $RADATTR ]; then
    # Get value of "Filter-Id" RADIUS-attribute.
    FILTER_ID=$(awk '/Filter-Id/ {print $2}' $RADATTR)
    if [ $FILTER_ID = $ACTIVE_FILTER_ID ]; then
      ipset del $BLOCK_SET_V4  $IPV4 -exist -quiet &> /dev/null
      ipset del $SOCIAL_SET_V4 $IPV4 -exist -quiet &> /dev/null
      ipset del $BLOCK_SET_V6  $IPV6_PREFIX -exist -quiet &> /dev/null
      ipset del $SOCIAL_SET_V6 $IPV6_PREFIX -exist -quiet &> /dev/null
      ipset del $BLOCK_SET_V6  $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
      ipset del $SOCIAL_SET_V6 $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
      logger -t ip-change "Allowed: IPv4 $IPV4, IPv6 $IPV6_PREFIX, IPv6-DP $IPV6_DELEGATED_PREFIX"
    elif [ $FILTER_ID = $BLOCK_FILTER_ID ]; then
      ipset del $SOCIAL_SET_V4 $IPV4 -exist -quiet &> /dev/null
      ipset add $BLOCK_SET_V4  $IPV4 -exist -quiet &> /dev/null
      ipset del $SOCIAL_SET_V6 $IPV6_PREFIX -exist -quiet &> /dev/null
      ipset add $BLOCK_SET_V6  $IPV6_PREFIX -exist -quiet &> /dev/null
      ipset del $SOCIAL_SET_V6 $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
      ipset add $BLOCK_SET_V6  $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
      logger -t ip-change "Blocked: IPv4 $IPV4, IPv6 $IPV6_PREFIX, IPv6-DP $IPV6_DELEGATED_PREFIX"
    elif [ $FILTER_ID = $SOCIAL_FILTER_ID ]; then
      ipset del $BLOCK_SET_V4  $IPV4 -exist -quiet &> /dev/null
      ipset add $SOCIAL_SET_V4 $IPV4 -exist -quiet &> /dev/null
      ipset del $BLOCK_SET_V6  $IPV6_PREFIX -exist -quiet &> /dev/null
      ipset add $SOCIAL_SET_V6 $IPV6_PREFIX -exist -quiet &> /dev/null
      ipset del $BLOCK_SET_V6  $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
      ipset add $SOCIAL_SET_V6 $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
      logger -t ip-change "Social: IPv4 $IPV4, IPv6 $IPV6_PREFIX, IPv6-DP $IPV6_DELEGATED_PREFIX"
    fi
  else
    logger -t ip-change "radattr file not found, $CALLED_SID $CALLING_SID"
  fi

Example /etc/accel-ppp_ip-down.sh script:

.. code-block:: sh

  #!/bin/sh

  # Option "Blocked".
  BLOCK_SET_V4='blk_usr_v4'
  BLOCK_SET_V6='blk_usr_v6'

  # Option "Social".
  SOCIAL_SET_V4='soc_usr_v4'
  SOCIAL_SET_V6='soc_usr_v6'

  # argv[5], contains IPv4-address,
  # (https://github.com/xebd/accel-ppp/blob/master/accel-pppd/extra/pppd_compat.c).
  IPV4=$5

  # Delete customer's IPv4|Pv6 addresses from all ipsets,
  # $IPV6_PREFIX and $IPV6_DELEGATED_PREFIX are environment variables from Accel-ppp,
  # (https://github.com/xebd/accel-ppp/blob/master/accel-pppd/extra/pppd_compat.c).
  ipset del $BLOCK_SET_V4  $IPV4 -exist -quiet &> /dev/null
  ipset del $SOCIAL_SET_V4 $IPV4 -exist -quiet &> /dev/null
  ipset del $BLOCK_SET_V6  $IPV6_PREFIX -exist -quiet &> /dev/null
  ipset del $SOCIAL_SET_V6 $IPV6_PREFIX -exist -quiet &> /dev/null
  ipset del $BLOCK_SET_V6  $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
  ipset del $SOCIAL_SET_V6 $IPV6_DELEGATED_PREFIX -exist -quiet &> /dev/null
  logger -t ip-change "Removing from all ipsets: IPv4 $IPV4, IPv6 $IPV6_PREFIX, IPv6-DP $IPV6_DELEGATED_PREFIX"

Example iptables/ipv6tables rules:

.. code-block:: sh

  iptables -t filter -A FORWARD -m set --match-set blk_usr_v4 src -m set ! --match-set blk_res_v4 dst -j DROP
  iptables -t filter -A FORWARD -m set --match-set soc_usr_v4 src -m set ! --match-set soc_res_v4 dst -j DROP
  iptables -t filter -A FORWARD -m set ! --match-set blk_res_v4 src -m set --match-set blk_usr_v4 dst -j DROP
  iptables -t filter -A FORWARD -m set ! --match-set soc_res_v4 src -m set --match-set soc_usr_v4 dst -j DROP

  ip6tables -t filter -A FORWARD -m set --match-set blk_usr_v6 src -m set ! --match-set blk_res_v6 dst -j DROP
  ip6tables -t filter -A FORWARD -m set --match-set soc_usr_v6 src -m set ! --match-set soc_res_v6 dst -j DROP
  ip6tables -t filter -A FORWARD -m set ! --match-set blk_res_v6 src -m set --match-set blk_usr_v6 dst -j DROP
  ip6tables -t filter -A FORWARD -m set ! --match-set soc_res_v6 src -m set --match-set soc_usr_v6 dst -j DROP
