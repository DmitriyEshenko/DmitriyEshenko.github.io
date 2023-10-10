.. _install_centos:

Install on Centos
-----------------

For compile with modules **vlan_mon** and **ipoe** on centos need install vanilla linux kernel or :ref:`elrepo_kernel_inst` . If that not needed, just set **-DBUILD_IPOE_DRIVER=FALSE** and **-DBUILD_VLAN_MON_DRIVER=FALSE** on cmake.

**Preparation**

Before compile and build package need satisfy some dependencies

* **rpm-build** - open-source system that manages the build process
* **cmake** - open-source system that manages the build process
* **gcc** - GNU Compiler Collection (GCC) is a compiler system
* **git** - version-control system for tracking changes, (need for downloading source code) 
* **pcre-devel** - source code of pcre lib, accel-ppp need it for use reg expression
* **openssl-devel** - source code of lib ssl, accel-ppp need it for use regular expression
* **lua-devel** - this need for create custom username (IPoE) from packet. Script write on lua language 

.. code-block:: sh

  yum -y install rpm-build make cmake gcc git pcre-devel openssl-devel lua-devel

After install dependencies, download accel-ppp source code with git client, tree master . Master tree contain actual patches last release. 

.. code-block:: sh

  git clone https://github.com/accel-ppp/accel-ppp.git /opt/accel-ppp-code

Create directory for build source code and go to this directory. 

.. code-block:: sh

  mkdir /opt/accel-ppp-code/build
  cd /opt/accel-ppp-code/build/

For building code need we can set next params:

* **-DBUILD_IPOE_DRIVER=TRUE** include IPoE module.This module need if you want use accel-ppp as shared interface.
* **-DBUILD_VLAN_MON_DRIVER=TRUE** include vlan monitoring module. If you want create vlan automatically on analyse IP headers with regular expression set on accel-ppp config file. (Available for IPoE and PPPoE)
* **-DKDIR=/usr/src/kernels/`uname -r`** sets path to Linux kernel source code. Need only for build IPOE, VLAN-MON.
* **-DCMAKE_INSTALL_PREFIX=/usr** path for install executable code. If you build DEB package, not recommended change this.
* **-DCPACK_TYPE=Centos7** this arguments for building RPM package. If used centos other version, set it.

.. code-block:: sh

  cmake -DBUILD_IPOE_DRIVER=TRUE -DBUILD_VLAN_MON_DRIVER=TRUE -DCMAKE_INSTALL_PREFIX=/usr -DKDIR=/usr/src/kernels/`uname -r` -DLUA=TRUE -DCPACK_TYPE=Centos7 ..

.. admonition:: Notice:

   ended symbols **..** sets path to accel-ppp source code, not delete this! Or you can replace it full path to accel-ppp-code like /opt/accel-ppp-code/
   
Compile:

.. code-block:: sh

  make 

Create RPM package:

.. code-block:: sh

  cpack -G RPM

Install package:

.. code-block:: sh

  rpm -ivh accel-ppp.rpm

If accel-ppp was build with modules **ipoe** and **vlan_mon**, need next:

.. code-block:: sh

  cp ./drivers/ipoe/driver/ipoe.ko /lib/modules/`uname -r`/kernel/net
  cp ./drivers/vlan_mon/driver/vlan_mon.ko /lib/modules/`uname -r`/kernel/net
  depmod -a

If you have success packet install, rename config file to accel-ppp.conf

.. code-block:: sh

  mv /etc/accel-ppp.conf.dist /etc/accel-ppp.conf
  
Edit accel-ppp.conf for you schemas and run accel-ppp

**Run as systemd unit:**

.. code-block:: sh

  systemctl start accel-ppp

or run manual (not recommended)

.. code-block:: sh

  accel-pppd -d -c /etc/accel-ppp.conf -p /var/run/accel-ppp.pid

.. toctree::
    :maxdepth: 2
    :caption: Contents:
    :includehidden:

    elrepo_kernel_inst.rst