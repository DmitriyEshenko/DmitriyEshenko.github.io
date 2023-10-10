[log]
=====

Configuration of log and log_file modules.

Config overview
^^^^^^^^^^^^^^^

**log-file=/path/to/file**
 By default is not defined. Required if used ``[modules]log_file``

 Path to file to write general log.

**log-emerg=/path/to/file**
 By default is not defined. Required if used ``[modules]log_file``
 
 Path to file to write emergency messages.

**log-fail-file=/path/to/file**
 By default is not defined.

 Path to file to write authentication failed session log.

**log-debug=/path/to/file**
 By default is not defined.

 Path to file to write all debug messages, also include mikrotime and threads numbers. 

**log-tcp=x.x.x.x:port**
 By default is not defined. Required if used ``[modules]log_tcp``

 Send logs to specified host. (Need add examples)

**syslog=ident[,facility]**
 By default is ``syslog=accel-pppd,daemon``

 Send logs to system logger. Facility may be: daemon, local0-local7 or numeric value.

**copy=0|1**
 By default is not defined.
 
 If this options is given, logging engine will duplicate session log in general log.  (Useful when per-session/per-user logs are not used).
 
**per-session-dir=dir**
 By default is not defined.

 Directory for session logs. If specified each session will be logged separately to file which name is unique session identifier.
 
**per-user-dir=dir**
 By default is not defined.
 
 Directory for user logs. If specified all sessions of same user will be logged to file which name is user name.

**per-session=0|1**
 By default is not defined.

 If specified then each session of same user will be logger separately to directory specified by "per-user-dir" and subdirectory which name is user name and to file which name os unique session identifier.

**level=n**
 By default is ``level=0``

 Specifies log level which values are:

 ``0`` turn off all logging
  
 ``1`` log only error messages
  
 ``2`` log error and warning messages

 ``3`` log error, warning and minimum information messages (use this level in conjuction with verbose option of other modules if you need verbose logging)

 ``4`` log error, warning and full information messages (use this level in conjuction with verbose option of other modules if you need verbose logging)
  
 ``5`` log all messages including debug messages


logs rotation
^^^^^^^^^^^^^

For rotation logs can be used system logrotate utility. Needs create file ``/etc/logrotate.d/accel-ppp`` and put next:

.. code-block:: sh
 
  /var/log/accel-ppp/*.log {
    missingok
    sharedscripts
    postrotate
      test -r /var/run/accel-pppd.pid && kill -HUP `cat /var/run/accel-pppd.pid`
    endscript
  }

.. admonition:: Note:

  For correct work *logrotate* utility need run ``accel-pppd`` daemon with ``-p /var/run/accel-pppd.pid`` argument.
  
.. Caution:: If accel-ppp run with gdb (GNU debugger) for find bugs, you need disable logs rotation, because it will makes to daemon crash.
