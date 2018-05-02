双网卡配置同一个网段的ip地址的问题

Linux 两块网卡相同的IP地址网段，默认情况无论连接哪个网卡，都连接只会从一个网卡出入。并且其他主机arp显示2个ip对应的MAC地址都是相同的。

    比linux主机的2个IP是：
    eth0 192.168.1.1/24 00-11-22-33-44-55
    eth1 192.168.1.2/24 55-44-33-22-11-00

    在linux主机arp -a显示
    ...
    192.168.1.1 00-11-22-33-44-55
    192.168.1.2 00-11-22-33-44-55
    ...
    OR
    ...
    192.168.1.1 55-44-33-22-11-00
    192.168.1.2 55-44-33-22-11-00
    ...

此刻查看默认路由表route -n

    ...
    0.0.0.0 192.168.1.1 eth0
    ...
    OR
    ...
    0.0.0.0 192.168.1.2 eth1
    ...

解决方法

给2个网卡配置不通的路由表

echo "220    local220" >> /etc/iproute2/rt_tables

echo "222    local222" >> /etc/iproute2/rt_tables

ip route add 192.168.12.0/24 dev eth0 src 192.168.12.220 table local220

ip route add 192.168.12.0/24 dev eth1 src 192.168.12.222 table local222

ip route add default dev eth0 table local220

ip route add default dev eth1 table local222

ip rule add from 192.168.12.220 table local220

ip rule add from 192.168.12.222 table local222

ip route flush cache

解释：

echo "220    local220" >> /etc/iproute2/rt_tables

    把路由表序号和对应的路由表名字添加到/etc/iproute2/rt_tables中

ip route add 192.168.12.0/24 dev eth0 src 192.168.12.220 table local220

    从192.168.12.220发送到192.168.12.0/24网段的数据从eth0发出，把该路由项添加到路由表local220中

ip route add default dev eth0 table local220

    在路由表中添加默认路由，默认路由从eth0进出

ip rule add from 192.168.12.220 table local220

    添加路由策略。来自 192.168.12.220的路由要求使用路由表local220

ip route flush cache

    把新添加的路由策略和路由表刷新到缓存中，即时生效


分析

问题在路由表，默认路由通常会被设置成为后启动的网卡。


补充两个细节，很多都会发现，当 Linux 有两个网卡地址，并且在同一个网段的时候，哪怕其中一个网卡不插网线，没网线的 ip 地址也可以被 ping 通。这是为什么呢？因为 Linux 默认的 sysctl 规则，任意一个网卡会对自己所有的 ip 地址在 ARP 请求上作出响应。当我们将两个网卡配置在同一网段后，最好给 sysctl.conf 加上这样的配置：


    net.ipv4.conf.all.arp_announce = 2
    net.ipv4.conf.all.arp_ignore = 1
    net.ipv4.conf.default.accept_source_route = 0
    net.ipv4.conf.default.arp_announce = 2
    net.ipv4.conf.default.arp_ignore = 1
    net.ipv4.conf.default.rp_filter = 1
    net.ipv4.conf.eth0.arp_announce = 2
    net.ipv4.conf.eth0.arp_ignore = 1
    net.ipv4.conf.eth1.arp_announce = 2
    net.ipv4.conf.eth1.arp_ignore = 1

另外Ping命令指定ip地址出口

    ping -I 192.168.1.1



