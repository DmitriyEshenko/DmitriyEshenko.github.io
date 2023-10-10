[radius]
========

**verbose=0|1**
  By default is not defined.

  If this option enabled, the radius module should add detailed info to log

**interim-verbose=0|1**
  By default is not defined.
  
  Specified, should radius module produce verbose logging of interim radius packets.

**dictionary=/path/to/dictionary**
  By default is `< -DCMAKE_INSTALL_PREFIX >/share/accel-ppp/radius/dictionary`
  
  Specifies path to RADIUS dictionaries. It is possible to define multiple dictionaries

**server=address,secret[,auth-port=1812][,acct-port=1813][,req-limit=0][,fail-timeout=0,max-fail=0,][,weight=1][,backup]**
  By default is not defined.

  Specifies IP address, secret, ports of RADIUS server.

**nas-ip-address=x.x.x.x**
  By default is not defined.

  Specifies value to send to RADIUS server in *NAS-IP-Address* attribute and to be matched in DM/CoA requests. Also DM/CoA server will bind to that address.

**nas-identifier=identifier**
  By default is not defined.

  Specifies value to send to RADIUS server in NAS-Identifier attribute and to be matched in DM/CoA requests.

**gw-ip-address=x.x.x.x**
  By default is not defined.

  Specifies address to use as local address of ppp interfaces if *Framed-IP-Address* received from RADIUS server.

**acct-interim-interval=n**
  By default is not defined.

  Specifies interval in seconds to send accounting information (may be overridden by radius *Acct-Interim-Interval* attribute)

**acct-interim-jitter=n**
  By default is not defined.
  
  Specifies absolute maximum jitter value in seconds to be applied to accounting information interval. Calculate interim-interval+-acct-interim-jitter.

**max-try=n**
  By default is ``max-try=3``

  Specifies number of tries to send Access-Request/Accounting-Request queries.

**timeout=n**
  By default is ``timeout=3``

  Timeout in seconds to wait response from RADIUS server.

**acct-timeout=n**
  By default is ``acct-timeout=3``

  Specifies timeout in seconds of accounting interim update, if request not received after this time , session will terminated. If ``acct-timeout=0`` then session keeps active.
  
**sid-in-auth=0|1**
  By default is not defined. 
  
  Specifies should *accel-ppp* generate and send ``Acct-Session-Id`` on Access-Request packet. By default ``Acct-Session-Id`` sent on Accounting-Request packet.
  
**acct-delay-time=0|1**
  By default is `acct-delay-time=0`
  
  Specifies whether radius client should include Acct-Delay-Time attribute to accounting requests

**attr-tunnel-type=name**
  By default is not defined. 
  
  Specifies custom attribute name to be used to send tunnel type (as string).

**default-realm=realm**
  By default is disabled.

  Append specified realm to username. For example ``default-realm=example.com`` accel-ppp send to RADIUS server ``username@example.com``

DM/CoA
^^^^^^

**dae-server=x.x.x.x:port,secret**
  By default is not defined.
  
  Specifies IP address, port to bind and secret for Dynamic Authorization Extension server (DM/CoA).This *ip address* must exist on any server interface.

**require-nas-identification=0|1**
  By default is not defined.
  
  Allow processing (DM/CoA) packets that contain valid "NAS-Identifier" and "NAS-IP-Address" attributes.
