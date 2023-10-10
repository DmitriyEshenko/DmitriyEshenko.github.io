[l2tp]
======

Overview configuration of L2TP module.

**verbose=0|1**
  By default is disabled.

  Specified will pptp module produce verbose logging.

**bind=x.x.x.x**
  By default bound on all IP address.

  If this option is given then *l2tp* server will bind to specified IP address.

**port=n**
  By default ``port=1701``

  If this option is given then *l2tp* server will bind to specified port.

**host-name=string**
  By default ``host-name=accel-ppp``

  This name will be sent to clients in Host-Name attribute.

**hello-interval=n**
  By default ``hello-interval=60``
  
  Specifies interval in seconds to send Hello control message. Its used for keep alive connection. If peer will not respond to *Hello* connection will be terminated.

**recv-window=n**
  By default ``recv-window=16`` Available value range 1-32768.

  Set the size of the local receive window. Only received messages whose sequence number is in the range [last-Nr + 1, last-Nr + recv-window] are accepted (where last-Nr is the sequence number of the last acknowledged message).

**timeout=n**
  By default ``timeout=60``

  Specifies timeout in seconds to wait peer completes tunnel and session negotiation.

**rtimeout=n**
   By default ``timeout=1``

  Specifies timeout (in seconds) to wait message acknowledge, if elapsed message retransmission will be performed. Timeout is multiplied by two after each retransmission. So if rtimeout is set to 1, first retransmission will occur after one second, second retransmission two seconds later, third one four seconds later, and so on, until a reply is received or the retransmit value is reached.

**rtimeout-cap=n**
  By default ``rtimeout-cap=16``

  Set the maximum interval between retransmissions. The exponential backoff interval used by rtimeout will never grow above rtimeout-cap. rtimeout-cap must be higher than rtimeout and, according to RFC 2661, must be no less than 8 (though accel-ppp doesn't enforce this rule).

**retransmit=n**
  By default ``retransmit=5``

  Specifies maximum number of message retransmission, if exceeds connection will be terminated.

**mppe=deny|allow|prefer|require**
  By default is not defined.

  Default behavior - don’t ask client for mppe, but allow it if client wants.

**secret=string**
  By default is not defined.

  Specifies secret to connect to server.

**hide-avps=0|1**
  By default ``hide-avps=0``
  
  If this option is given and ``hide-avps=1``, then attributes sent in L2TP packets will be hidden (for AVPs that support it).

**dataseq=deny|allow|prefer|require**
  By default ``dataseq=allow``

  Specify data sequencing negotiation algorithm: 
  
  * deny - don't send data packets with sequence numbers
  
  * allow - send data packets with sequence numbers if peer have requested so only 

  * prefer - send data packets with sequence numbers and enable same for peer 

  * require - send data packets with sequence numbers and enforce same for peer

**reorder-timeout=n**
  By default ``reorder-timeout=0``

  Specifies timeout in milliseconds to wait for out-of-order packets. If *0*, don't try to reorder.

**use-ephemeral-ports=0|1**
  By default ``use-ephemeral-ports=0``

  Specifies if an arbitrary source port is used when replying to a tunnel establishment request. When this option is deactivated, the destination port of the incoming request (SCCRQ) is used as source port for the reply (SCCRP).

**ppp-max-mtu=n**
  By default ``ppp-max-mtu=1420``

  Set the maximum MTU value that can be negotiated for PPP over L2TP sessions.

**ifname=ifname**
  By default is not defined.

  If this option is given ppp interface will be renamed using ifname as a template, i.e ``ifname=l2tp%d`` => l2tp0.
  
.. admonition:: Note:

    Also interface may renamed if RADIUS server send attribute ``NAS-Port-Id`` with custom name. Length this value not be more 16 characters.

**avp_permissive=0|1**

**dir300_quirk=0|1**

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
