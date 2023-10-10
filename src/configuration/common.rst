[common]
========

Contains common params for all connection types


**single-session=replace|deny**
  By default is not defined.

  Specifies whether accel-ppp should control sessions count. If this option is absent session count control is turned off. If this option is ``replace`` then accel-ppp will terminate first session when second is authorized. If this option is ``deny`` then accel-ppp will deny second session authorization.
  
**sid-case=upper|lower**
  By default is ``sid-case=lower``

  Specifies in which case generate session identifier (attribute ``Acct-Session-Id``).

**sid-source=urandom|seq**
  By default ``sid-source=urandom``
  
  Specifies method assign session id.
  
  * ``urandom`` - assign session id by random method
  * ``seq`` - assign session id by sequence method

**seq-file=path**
  By default is ``seq-file=/var/lib/accel-ppp/seq``
  
  Path to file for sessions sequence number. Start sequence number may be set there (default /var/lib/accel-ppp/seq).

**max-sessions=n**
  By default is disabled ``max-sessions=0``
  
  Specifies maximum sessions which server may processed. After reaching ``max-sessions`` accel-ppp will ignore connection tries for new sessions.

**max-starting=n**
  By default is disabled ``max-starting=0``
  
  Specifies maximum concurrent session attempts which server may processed.

**check-ip=0|1**
  y default is: ``check-ip=0``

  Specifies whether accel-ppp should check if IP already assigned to other ppp or ipoe interface.
  
**netns-run-dir=/path/to/netns**
  By default: ``netns-run-dir=/var/run/netns``
  
  Specifies path where accel-ppp should find netns objects
  
