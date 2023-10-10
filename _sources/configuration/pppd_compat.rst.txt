[pppd-compat]
=============

Configuration of pppd_compat module. Often used for creation custom shaper or other custom tricks.
This module starts pppd compatible ip-up/ip-down scripts and ip-change to handle RADIUS CoA request.

Config overview
^^^^^^^^^^^^^^^
**verbose=0|1**
  Default value is ``verbose=0``

  If specified and greated then 0, pppd_module will produce verbose logging.

**radattr-prefix=/path**
  By default is not defined.

  Specifies prefix of radattr files (for example ``radattr=/var/run/radattr``, resulting files will be ``/var/run/radattr.pppX``)

**ip-pre-up=/path/to/file**
  By default is not defined.

  Path to ip-pre-up script which is executed before ppp interface comes up, useful to setup firewall rules before any traffic can pass through the interface.

**ip-up=/path/to/file**
  By default is not defined.

  Path to ip-up script which is executed when ppp interfaces is completely configured and started.

**ip-down=/path/to/file**
  By default is not defined.

  Path to ip-down script which is executed when session is about to terminate.

**ip-change=/path/to/file**
  By default is not defined.
  
  Path to ip-change script which is executed for RADIUS CoA handling.

**fork-limit=n**
  By default is calculated ``threads*2``
  
  Specifies number of simultaneously running background processes. For disable this feature need set ``fork-limit=0``
