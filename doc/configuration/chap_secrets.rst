[chap-secrets]
==============

*Chap-secret* is the module of authentication which works with user authentication data and other data (username, password, ip address, speed etc.) stored as local file. Currently *accel-ppp* may works only with one of the authentication method, chap-secrets or RADIUS. RADIUS has more priority if set in ``[modules]`` section. Remove or *#comment* ``radius`` from section ``[modules]`` if you want use ``chap-secrets``. Example:

.. code-block:: sh

    [modules]
    chap-secrets
    #radius

Configuration
-------------

**chap-secrets=/path/to/file**
    By default is ``chap-secrets=/etc/ppp/chap-secrets``
    
    Specifies alternate chap-secrets file location.

**username-hash=hash1[,hash2]**
    By default is not defined.

    Specifies hash chain to calculate username hash. hash1, hash2 are openssl known digest names (md5, sha1, etc).
    For example, ``username-hash=md5,sha1`` means hash username through md5 and then binary result hash through sha1.
    Username have to be specified as hexadecimal dump of digest result.Password field have to be encrypted using smbencrypt (NT Hash part).

**encrypted=0|1**
    By default is disabled: ``encrypted=0``

    Specifies either chap-secrets is encrypted.

.. admonition:: Note:

    Encryption is incompatible with auth_chap_md5 module.
    
    To enable chap-secrets encryption ablity accel-ppp must be compiled with -DCRYPTO=OPENSSL (which is default).

**gw-ip-address=x.x.x.x[/mask]**
    By default is not defined.

    Specifies address to use as local address of ppp interfaces if chap-secrets is used for IP address assignment. Mask is used for IPoE.

Chap-secrets file example
-------------------------

.. code-block:: sh

    #client     server      secret      ip-address      speed
    user001     *           password1	100.64.100.1	20480/10240
    user002     *           passowrd2	*               10240/10240
    user003     *           passowrd3	ip_pool1        10240
    eth0.101    *           eth0.101    ipoe_pool       20480
    100.64.0.2  *           100.64.0.2  *               
    
* The first column contain *username*.
* The second column is only keep for support chap secrets files standard.
* The third column contain secret or password.
* The fourth column may contain allocated ip address or pool name which configured in ``[ip-pool]`` section.
* The fifth column contain rate-limit.
