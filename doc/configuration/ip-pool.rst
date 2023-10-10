[ip-pool]
=========

Configuration of ippool module.

**gw-ip-address=x.x.x.x**

  Specifies single IP address to be used as local address of ppp interfaces.

**gw=range**
  
  Range format should be ``x.x.x.x/mask`` or ``x.x.x.x-y``
  
  Specifies range of local address of ppp interfaces.

**tunnel=range**

  Specifies range of remote address of ppp interfaces, format is same as above. 
  
**x.x.x.x/mask,pool_name | x.x.x.x-y,pool_name**
  
  Also specifies range of remote address of ppp interfaces.
