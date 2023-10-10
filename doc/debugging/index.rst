.. _debugging:

Debugging
=========

Sometimes for debugging need to build accel-ppp with additional flags:

``-DCMAKE_BUILD_TYPE=Debug`` - Include debug information to accel-pppd binary

``-DCMAKE_C_FLAGS='-g -O0'`` - Enable optimization flags

``-DMEMDEBUG=TRUE`` - Set this flag if you want to debug memleak

Allow create core dump files without size limiting, edit /etc/security/limits.conf and add

.. code-block:: sh
  
  *  soft  core  unlimited

Or run ``ulimit -c unlimited`` for apply immediately

Edit ``/etc/sysctl.conf`` to define core dump file name and location, add

.. code-block:: sh
  
  kernel.core_uses_pid = 1
  kernel.core_pattern = /root/core-%e-%p

And apply ``sysctl -p``

If accel-ppp runs as systemd service to allow create coredumps it is necessary to add ``DefaultLimitCORE=infinity`` to ``/etc/systemd/system.conf`` and run ``systemctl daemon-reexec`` to activate new params.

Recommended: to create a self-checking program with predefined mistake. Create ``test.c`` with the following content

.. code-block:: sh
  
  int main() {
    *(char *)0 = 0;
    return 0;
  }

Compile this program ``gcc test.c`` and run ``./a.out``

If core files appear in ``/root`` directory and in ``dmesg`` exist output ``a.out[xxxx]: segfault at ...`` then all done properly.

**Run accel-ppp in GDB (GNU Debugger)**

If you want to run accel-ppp in GDB, necessary disable logratation for accel-ppp log files.

.. code-block:: sh
  
  gdb -ex=run  --args accel-pppd -c /etc/accel-ppp.conf -p /var/run/accel-ppp.pid
  
 
