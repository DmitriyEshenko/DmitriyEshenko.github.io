[pptp]
=======

Configuration overview of PPTP module.

**verbose=0|1**
  By default is disabled.

  Specified will pptp module produce verbose logging.

**bind=x.x.x.x**
  By default bound on all ip address.

  If this option is given then pptp server will bind to specified IP address.

**port=n**
  By default ``port=1723``

  If this option is given then pptp server will bind to specified port.

**echo-interval=n**
  By default is disabled ``echo-interval=0``

  If this option is given and greater then zero then pptp module will send echo-request every n seconds.

**echo-failure=n**
  By default ``echo-failure=3``

  Specifies maximum number of echo-requests may be sent without valid echo-reply, if exceeds connection will be terminated.

**timeout=n**
  By default ``timeout=5``

  Timeout waiting reply from client in seconds.

**mppe=deny|allow|prefer|require**
  By default is not defined.

  Default behavior - don’t ask client for mppe, but allow it if client wants.
  
**ifname=ifname**
  By default is not defined.

  If this option is given ppp interface will be renamed using ifname as a template, ``ifname=pptp%d`` => pptp0.
  
.. admonition:: Note:
    
  Also interface may renamed if RADIUS server send attribute ``NAS-Port-Id`` with custom name. Length this value not be more 16 characters.
    
**ppp-max-mtu=n**
  By default ``ppp-max-mtu=1436``

  Set the maximum MTU value that can be negociated for PPP over PPTP sessions.

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
