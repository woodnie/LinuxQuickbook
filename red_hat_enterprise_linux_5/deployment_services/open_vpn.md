### Open VPN {#open-vpn}

drwxr-xr-x 3 root    root      4096 May 12 00:35 temp

ip-172-31-15-53:~ # rpm -qil openvpn-as-2.0.17-openSUSE12.x86_64.rpm 

package openvpn-as-2.0.17-openSUSE12.x86_64.rpm is not installed

ip-172-31-15-53:~ # rpm -ivh openvpn-as-2.0.17-openSUSE12.x86_64.rpm    

Preparing...                          ################################# [100%]

Updating / installing...

   1:openvpn-as-0:2.0.17-openSUSE12.1 ################################# [100%]

The Access Server has been successfully installed in /usr/local/openvpn_as

Configuration log file has been written to /usr/local/openvpn_as/init.log

Please enter &quot;passwd openvpn&quot; to set the initial

administrative password, then login as &quot;openvpn&quot; to continue

configuration here: https://172.31.15.53:943/admin

To reconfigure manually, use the /usr/local/openvpn_as/bin/ovpn-init tool.

Access Server web UIs are available here:

Admin  UI: https://172.31.15.53:943/admin

Client UI: https://172.31.15.53:943/