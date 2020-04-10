### network {#network}

VORACLE11:~ # vi /etc/sysconfig/network/config 

## Type:        string

288 ## Default:    &quot;&quot;

289 #

290 # List of DNS domain names used for host-name lookup.

291 # It is written as search list into the /etc/resolv.conf file.

292 #

293 #NETCONFIG_DNS_STATIC_SEARCHLIST=&quot;&quot;

294 NETCONFIG_DNS_STATIC_SEARCHLIST=&quot;apauto.trw.com&quot;

295 

296 ## Type:        string

297 ## Default:    &quot;&quot;

298 #

299 # List of DNS nameserver IP addresses to use for host-name lookup.

300 # When the NETCONFIG_DNS_FORWARDER variable is set to &quot;resolver&quot;,

301 # the name servers are written directly to /etc/resolv.conf.

302 # Otherwise, the nameserver are written into a forwarder specific

303 # configuration file and the /etc/resolv.conf does not contain any

304 # nameservers causing the glibc to use the name server on the local

305 # machine (the forwarder). See also netconfig(8) manual page.

306 #

307 #NETCONFIG_DNS_STATIC_SERVERS=&quot;&quot;

308 NETCONFIG_DNS_STATIC_SERVERS=&quot;149.223.131.1 140.171.1.10&quot;

 #/sbin/netconfig  update

VORACLE11:~ # cat /etc/resolv.conf 

search apauto.trw.com

nameserver 149.223.131.1

nameserver 140.171.1.10