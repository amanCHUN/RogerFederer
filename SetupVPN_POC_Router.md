### 路由器（POC）VPN连接方法

update 2018/04/19 16:50 添加新的VPN客户端软件适用于win10

以下步骤适用于windows7

1 安装VPN客户端软件
Secospace VPN Client

链接:https://pan.baidu.com/s/11YKQ4BaEjk4ABf8LcZHNYQ  密码:1qm6

2 运行Secospace VPN Client，配置连接。

    第一步：请选择创建方法。点击新建进入“新建连接向导”，选择“通过输入参数创建连接”
    第二步：请输入登陆设置。输入LNS服务器地址（外网地址另见邮件）输入用户名和密码（另见邮件）
    第三步：请输入L2TP设置。隧道名称输入lac，认证模式选择CHAP。

3 如需连接成后允许访问Internet，则点击属性，勾选下边的“连接成功后允许访问Internet”

4 添加路由，由于VPN获得IP网段和内网设备IP不在同一个网段。点击属性——路由设置，点击添加输入IP地址和子网掩码。

比如需要访问10.110.255.1/16的server，那么需要添加IP地址为10.110.0.0，子网掩码255.255.0.0

PS：以上第2、3、4步骤可以通过导入配置完成。

目前在windows7中连接VPN成功，macOS系统需要使用l2tpnoipsec方式连接，但由于系统升级已经不能连接。

代替1中VPN客户端软件，最新软件连接如下：

链接:https://pan.baidu.com/s/1k3ru7XWDZuHo-aDiS_TAIg  密码:rv7z


