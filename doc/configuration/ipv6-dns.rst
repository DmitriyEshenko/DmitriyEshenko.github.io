[ipv6-dns]
==========

Overview ipv6 DNS section.

**dns=IPv6_address**
  By default is not defined.

  Specifies IPv6 DNS to be sent to peer. You may specify up to 3 dns options.

**dnssl=name**
  By default is not defined.
  
  Specify DNS Search List. You may specify multiple dns and dnssl options.
  
.. admonition:: Note:
    
    Also DNS addresses may be described like
 
 .. code-block:: sh

    [ipv6-dns]
    2001:4860:4860::8888
    2001:4860:4860::8844
