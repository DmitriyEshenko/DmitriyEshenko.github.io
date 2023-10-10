[core]
======

Section [core] consist main daemon params.

* **log-error=path** - path to file for core module error logging.
* **thread-count=n** - number of working threads, optimal - number of processors/cores.

.. admonition:: Note:

   Can’t change with reload, for apply changes need daemon restart with drop active sessions.

