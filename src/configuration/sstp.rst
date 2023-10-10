.. _sstp:

[sstp]
======

Configuration options of sstp module.

Configuration of SSTP module.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**bind=x.x.x.x|ipv6address|unix:pathname|unix:@abstract**
    If this option is given then sstp server will bind to specified IP address or unix pathname/abstract socket. 

**port=n**
    If this option is given then sstp server will bind to specified port. Default is 443. 

**verbose=n**
    If this option is given and n is greater of zero then sstp module will produce verbose logging. 

**timeout=n**
    Timeout waiting reply from client in seconds. Default is 60. 

**hello-interval=n**
    If this option is given and greater then zero then sstp will send echo-request every n seconds and drop connection without a reply. Default is 60. 

**accept=ssl,proxy**
    Specifies incoming connection acceptance mode.
    * **ssl** - enable SSL/TLS support.
    * **proxy** - enable PROXY protocol 1 & 2 support. 

**ssl-dhparam=pemfile**
    Specifies a file with DH parameters for DHE ciphers. 

**ssl-ecdh-curve=string**
    Specifies a curves for ECDHE ciphers. Value is specified in the format understood by the OpenSSL library. 

**ssl-ciphers=string**
    Specifies the enabled ciphers. The ciphers are specified in the format understood by the OpenSSL library. 

**ssl-prefer-server-ciphers=n**
    If this option is given and n is greater of zero then server ciphers should be preferred over client ciphers. Default is 0. 

**ssl-pemfile=pemfile**
    Specifies a file with the certificate in the PEM format for sstp server. Certificate is also used to compute initial SHA1 and SHA256 certificate hash. 

**ssl-keyfile=keyfile**
    Specifies a file with the secret key in the PEM format for sstp server. If not set, secret key will be loaded from the pemfile certificate. 

**cert-hash-proto=sha1,sha256**
    Specifies hashing methods that can be used to compute the Compound MAC in the Crypto Binding attribute. Default is sha1 and sha256 both.

**cert-hash-sha1=hexstring**
    Given hexadecimal value overrides SHA1 hash computed from the pemfile certificate or used directly for non-ssl mode. 

**cert-hash-sha256=hexstring**
    Given hexadecimal value overrides SHA256 hash computed from the pemfile certificate or used directly for non-ssl mode. 

**host-name=string**
    If this option is given, only sstp connection to specified host and with the same TLS SNI will be allowed. 

**http-error=deny|allow|http[s]://host.tld[/path]**
    Specify http layer error behavior for non-sstp requests.
    * **deny** - reset connection without any error response.
    * **allow** - respond with http-specific status codes.
    * **http[s]://host.tld[/path]** - respond with http redirect to the specified location. If /path is not specified, requested uri will be appended automatically
    Default value is allow. 

**ifname=ifname**
    If this option is given ppp interface will be renamed using ifname as a template, i.e `sstp%d => sstp0`. 

**ppp-max-mtu=n**
    Set the maximun MTU value that can be negociated for PPP over SSTP sessions. Default value is 1452, maximum is 4087.
