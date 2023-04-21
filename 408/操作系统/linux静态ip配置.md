## centos

```shell
TYPE=Ethernet
# PROXY_METHOD=none
# BROWSER_ONLY=no
#NM: PEERDNS = yes
#NM: PEERROUTES = yes
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens32
UUID=3c515747-24b8-47ce-be5c-2bb0ab60ffbe
DEVICE=ens32
ONBOOT=yes
# IPV6_PRIVACY=no

DNS1=114.114.114.114
ZONE=public
IPADDR=192.168.1.140
NETMASK=255.255.255.0
PREFIX=24
GATEWAY=192.168.1.1

IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
# CONNECTION_METERED=no


```

## ubuntu

interfaces

```shell
auto ens32
iface ens32 inet static
address 192.168.2.88   
netmask 255.255.255.0  
gateway 192.168.2.1    
dns-nameserver 114.114.114.114  
```



## settings

![image-20210309165253352](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202103/09/165254-433526.png)

