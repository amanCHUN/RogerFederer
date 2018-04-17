### 配置远程拨号用户发起L2TP隧道连接示例

此文档用于在华为AR121-S企业路由器中配置远程用户连接l2tpVPN，原文档连接在最下边。

组网需求

企业出差员工的地理位置经常发生变动，并且随时需要和总部通信和访问总部内网资源，直接通过Internet网络虽然可以访问总部网关，但总部网关无法对接入的用户进行辨别和管理，这时将总部网关部署为LNS，出差员工在PC终端上使用L2TP拨号软件，则可以在出差员工和总部网关之间建立虚拟的点到点连接。本示例在PC终端上以Windows7为例。

配置思路

配置远程拨号用户发起L2TP隧道连接的思路如下：

1.企业总部网关LNS接入Internet，作为LNS响应出差员工的L2TP请求。

2.出差员工接入Internet后，通过设置L2TP拨号软件，向LNS发起L2TP连接的请求。

操作步骤

步骤1配置LNS

配置公网IP地址及路由，假设访问公网路由的下一跳地址为202.1.1.2。

    <Huawei> system-view
    [Huawei] sysname LNS
    [LNS] interface gigabitethernet 1/0/0
    [LNS-GigabitEthernet1/0/0] ip address 202.1.1.1 255.255.255.0
    [LNS-GigabitEthernet1/0/0] quit
    [LNS] ip route-static 0.0.0.0 0 202.1.1.2

配置L2TP用户的用户名为huawei，密码为Huawei@1234，用户类型固定为ppp

    [LNS] aaa
    [LNS-aaa] local-user huawei password
    Please configure the login password (8-128)
    It is recommended that the password consist of at least 2 types of characters, including lowercase letters, uppercase letters, numerals and special characters. Please enter password: Please confirm password:Info: Add a new user.
    Warning: The new user supports all access modes. The management user access modes such as Telnet, SSH, FTP, HTTP, and Terminal have security risks. You are advised to configure the required access modes only.  
    [LNS-aaa] local-user huawei service-type ppp
    [LNS-aaa] quit

定义一个地址池，为拨入用户分配地址。

    [LNS] ip pool lns
    [LNS-ip-pool-lns] network 192.168.1.0 mask 24
    [LNS-ip-pool-lns] gateway-list 192.168.1.1
    [LNS-ip-pool-lns] quit

配置虚拟接口模板。

    [LNS] interface virtual-template 1
    [LNS-Virtual-Template1] ip address 192.168.1.1 255.255.255.0
    [LNS-Virtual-Template1] ppp authentication-mode chap
    [LNS-Virtual-Template1] remote address pool lns
    [LNS-Virtual-Template1] quit

使能L2TP功能，并创建L2TP组编号为1。

    [LNS] l2tp enable
    [LNS] l2tp-group 1


禁止隧道认证功能，Windows 7不支持隧道认证。

    [LNS-l2tp1] undo tunnel authentication

配置LNS绑定虚拟接口模板。

    [LNS-l2tp1] allow l2tp virtual-template 1

步骤2 配置Windows 7

参考文件

华为-L2TP-vpn配置详解.pdf

链接:https://pan.baidu.com/s/1ey4cM0oj6MHIPHnB_Qpukg  密码:gsgf
