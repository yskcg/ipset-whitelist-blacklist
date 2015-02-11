# ipset-whitelist-blacklist
ipset the white/black list for the device that connetc to the router and ip is in lan range,generate by dhcp.
##authd 认证/白/黑名单
**1. cat /etc/firewall.auth；**
>

**2. ipset list; **
>

**3. ubus call auth get; ubus list -v auth**

**4. 实现的功能**
>
>1. 实现web网页的认证的开启与关闭开关;
>
>2. 实现在线用户列表;
>
>3. 实现白名单(dhcp 分配的内网ip地址)
>
>4. 实现黑名单(dhcp 分配的内网ip地址)

###FAQ

>1. 在线用户列表，怎样唯一sure,ip+mac;
>
>2. 黑白名单默认的时候均为空，用户需要从在线用户列表中去取值进行设置；
>
>3. 认证后列表和白名单的作用是相同的；

##原理
>**在线用户列表**
>
>>1. 提供点击页面自动更新，以及reflash 按钮更新；
>
>>2. 后台，调用脚本来实现从arp 表中读取ip 地址和mac，匹配dhcp realease 中的动态ip地址，以及通过网页来静态绑定的地址，或者用户自己设定的静态地址；
>
>>3. 匹配后的ip 地址保存到uci 的config 文件中，以方便网页调用；

>**黑名单**
>
>>1. 用户点击网页，从用户在线列表中选取设备设置黑名单，luci 收到该消息后，需要进行两个动作：
>
>>>a. 保存该配置，uci 类型的配置文件；
>
>>>b. 调用脚本进行iptables,ipset 的设置；
>>>>1. 脚本实现更新iptables 规则表；
>
>>>c. 更新用户在线列表；
>
>>2. 开机启动时，需要去读取DHCP的配置文件，来进行解析黑白名单，进行防火墙规则的改变
>
>>3. 页面进行配置的时候，怎么来去调用脚本来进行处理呢？
>
>>4. 认证的开关，可以使用如下的命令来操作
>>>关闭认证：`ubus call auth status '{"enable":false}'`
>>>开启认证： `ubus call auth status '{"enable":true}'`
