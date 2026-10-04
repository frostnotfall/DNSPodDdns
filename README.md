# DNSPodDdns

对荒野无灯基于[anrip](https://github.com/anrip/ArDNSPod)的脚本添加 ipv6 支持
* 2019.01.21 - 由于梅林获取ipv6有延迟，修改了下脚本，只在ddns ipv4 更新后通知梅林成功，更新ipv6后不会通知。当然实际上已经可用了
* 2020.12.07 - 由于部分固件的wget版本过旧导致无法连接dnspod，故改为curl
* ​                     - 获取ipv6的方式从ppp0改为br0，匹配`240x:xxxx:........`

荒野无灯原帖：http://koolshare.cn/thread-37553-1-1.html

主要做了以下几点修改：
1. arNslookup （查询域名已解析的IP地址 ）修改为 dnspod api post方式
2. 梅林固件重启后不会立刻获取到公网 ipv6 地址，当脚本检测不到 ipv6 地址时会等待 5 分钟后重新获取，还是没有则继续等待 5 分钟 ，以此类推。 期间梅林控制台页面会显示叹号，但此时 ipv4 动态解析已经可用 
3. 域名，二级域名 的传入参数作为全局变量使用，相关函数的传入参数做了简化

使用此脚本，需：
1. 指定 arToken ，Token请去 https://www.dnspod.cn/console/user/security 获取
2. 指定 arDdnsCheck "域名" "二级域名(可为空)" 如：
arDdnsCheck "baidu.com" ""  会动态解析 baidu.com 域名
arDdnsCheck "baidu.com" "www"  会动态解析 www.baidu.com 域名
3. 因为 ipv4 与 ipv6 使用不同的recordID，使用脚本前先在 dnspod 官方控制台页面添加该域名（或二级域名）的 AAAA 记录，以生成 ipv6 的recordID
