### ip {#ip}

ip

NAME

      ip - show / manipulate routing, devices, policy routing and tunnels

[root@node ~]# ip link set DEVICE

[root@node ~]# ip link show [ DEVICE ]

[root@node ~]# ip addr { show | flush }

[root@node ~]# ip addr { add | del } IFADDR dev STRING

[root@node ~]# ip route add to 172.16.201.0/24 via 172.16.200.1