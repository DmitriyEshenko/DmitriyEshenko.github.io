.. _shaper:

[shaper]
========
Accel-ppp support many ways customisation rate-limit. Also limiting clients bandwidths sometimes called as QoS (Quality of Service), but QoS has more possibilities. For enable rate-liniter, we can sets ``shaper`` in section ``[modules]``.

Config overview
---------------

**attr=name**
  By default: ``attr=Filter-Id``.
  
  Specifies which radius attribute contains rate information. RADIUS server can transmit ``Filter-Id=1000``, means 1000Kbit both up-stream and down-stream rate or ``Filter-Id=2000/3000``, means 2000Kbit down-stream rate and 3000Kbit up-stream rate.

**attr-up=name**
  By default is not defined.
  
  Specifies which radius attribute contains rate information for **upstream**. Often used if needs separate *upstream* and *downstream* attributes.

**attr-down=name**
  By default is not defined.

  Specifies which radius attribute contains rate information for **downstream**. Often used if needs separate *upstream* and *downstream* attributes.

**vendor=name**
  By default is not defined.

  Specifies vendor name  for support attributes of other vendors like *Cisco-AVPair* or *Mikrotik*.
  
Example for Cisco:
  
.. code-block:: sh
 
  vendor=Cisco
  attr=Cisco-AVPair

Example for Mikrotik:

.. code-block:: sh
 
  vendor=Mikrotik
  attr=Mikrotik-Rate-Limit
  
**burst-factor=n**
  By default is not defined.
  
  Burst will be calculated as rate multiply burst-factor. Common ``burst-factor`` for upstream calculated as ``burst-factor*10``.

**up-burst-factor=n**
  By default is ``up-burst-factor=1``
  
  Specifies burst factor for **upstream**.

**down-burst-factor=n**
  By default is ``down-burst-factor=0.1``

  Specifies burst factor for **downstream**.

**cburst=n**
  By default is ``cburst=1534``

  Specifies amount of bytes that can be burst at 'infinite' speed. Recommendation: cburst should be equal to at most one average packet 

**latency=n**
  By default is ``latency=0.05``

  Specifies latency (in milliseconds) parameter of tbf qdisc which set maximum amount of time a packet can sit in the TBF.

**mpu=n**
  By default is ``mpu=0``

  Specifies mpu parameter in bytes of tbf qdisc and policer. Determines the minimal token usage for a packet.

**r2q=n**
  By default is ``r2q=10``

  Specifies r2q parameter of root htb qdisc.

**quantum=n**
  By default is ``quantum=1500``

  Specifies quantum parameter of htb classes. Amount of bytes a flow is allowed to dequeue before the scheduler moves to the next class.

**moderate-quantum=1|0**
  By default is disabled ``moderate-quantum=0``

  If fixed quantum is not specified and this option is specified then shaper module will check for quantum value is valid (in range 1000-200000).

**fwmark=n**
  By default is disabled: ``fwmark=0``

  Specifies the fwmark for traffic that won't be passed through shaper.

**up-limiter=police|htb**
  By default is: ``up-limiter=police``

  Specifes upstream rate limiting method.

**down-limiter=tbf|htb**
  By default is: ``down-limiter=tbf``

  Specifies downstream rate limiting method.

**ifb=ifb_ifname**
  By default ``ifb=ifb0``
  
  Specifies name of ifb interface, used only for ``up-limiter=htb``
  
**leaf-qdisc=qdisc parameters**
  By default is not defined.

  In case if htb is used as up-limiter or down-limiter specified leaf qdisc can be attached automaticaly. At present *sfq* and *fq_codel qdiscs* are implemented. *CoDel* (the name comes from "controlled delay") is Active Queue Manager. Parameters are same as for tc: 
  
  ``sfq [limit NUMBER] [perturb SECS] [quantum BYTES]``

  ``fq_codel [limit PACKETS] [flows NUMBER] [target TIME] [interval TIME] [quantum BYTES] [[no]ecn]``
  
**rate-multiplier=n**
  By default is ``rate-multiplier=1``

  Due to accel-ppp operates with rates in kilobit basis if you send rates in different basis then you can use this option to bring your values to kilobits. For ``vendor=Mikrotik`` often sets ``rate-multiplier=0.001``
 
**rate-limit=downstream/upstream**
  By default is not defined.

  Specifies default speed if there are no radius attributes.

**time-range=range_id,time_start-time_end**
  By default is not defined.

  Specifies time ranges for automatic rate reconfiguration. You can specify multiple such options.
  
  Configuration example:

.. code-block:: none
 
  [shaper]
  time-range=1,1:00-3:00
  time-range=2,3:00-5:00
  time-range=3,5:00-7:00

The first number is time range identifier. To define a specific rates uses following format of radius attributes: range-id,rate, range-id,down-rate/up-rate or cisco-like.

As an example:

.. code-block:: none

  Filter-Id=1000
  Filter-Id=1,2000
  Filter-Id=2,3000
  Filter-Id=3,4000

That means: set 1000Kbit by default, set 2000Kbit in time range 1, set 3000Kbit in time range 2 and set 4000Kbit in time range 3.
You have to pass multiple Filter-Id attributes to utilize this functionality.


Examples
--------

Fiter-Id
--------

Cisco AVPair
^^^^^^^^^^^^^^

Mikrotik
^^^^^^^^
