.. _lua_examples:

Lua examples
============

Important that accel-ppp was built with lua support ``cmake -DLUA=TRUE`` or if system has more modern lua version, need this sets, for example ``cmake -DLUA=5.3``

Example accel-ppp configuration:

.. code-block:: sh

  [ipoe]
    lua-file=/etc/accel-ppp.lua
    username=lua:username_func

Create /etc/accel-ppp.lua and edit. Example for D-link switches with Option 82:

.. code-block:: sh

  #!lua
    function username_func(pkt)
      v,b1,b2,b3,b4=string.unpack(pkt:agent_remote_id():sub(-4),'bbbb')
      ip=b1..'.'..b2..'.'..b3..'.'..b4
      v,port=string.unpack(string.sub(pkt:agent_circuit_id(),'-1'),'b')
      local username=ip..'-'..port
  --  print(username)
      return username
  end

Object **pkt** has next functions:

**hdr(name)**
  Will return value which contained in DHCP packet header. ``name`` may receive next params: ``xid``, ``ciaddr``, ``giaddr``, ``chaddr``.
 
**ifname()**
  Will return interface name which received packet.

**ipaddr()**
  Will return client ip address exist in packet header.

**hwaddr()**
  Will return client MAC address.

**vlan()**
  Will return client VLAN.

.. code-block:: sh

  local vlan = pkt:vlan()
  local svid = bit.rshift(vlan,16)
  local cvid = bit.band(vlan,0xffff)

**options()**
  Will return table which contains number of DHCP option in received packet.

**option(num)**
  Will return value with option number ``num``.

**agent_circuit_id()**
  Will return ``agent_circuit_id`` option 82.

**agent_remote_id()**
  Will return ``agent_remote_id`` option 82.

.. admonition:: Note:

    All function return type ``string``, except for ``options()``

Also to accel-ppp includes packet **lpack** for disassemble binary data.
It add to object ``string`` additional function ``unpack(binary, fmt)``, where ``binary`` is string which contain binary data, and ``fmt`` is data format. To ``fmt`` may be sets next data types:

**z** - zero terminated string

**p** - string precended by length byte

**P** - string precended by length word

**f** - float

**d** - double

**c** - int8_t

**b** - uint8_t

**h** - int16_t

**H** - uint16_t

**i** - int32_t

**I** - uint32_t

**l** - int64_t

**L** - uint64_t

**<** - little endian

**>** - big endian

**=** - native endian
