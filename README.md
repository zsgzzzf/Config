# OpenClash从入门到精通

immortalwrt固件：[点击这里](https://firmware-selector.immortalwrt.org/)

openclash luci地址：[点击这里](https://github.com/vernesong/OpenClash)

openclash核心地址：[点击这里](https://github.com/vernesong/OpenClash/tree/core/master)

clashmeta官方核心地址：[点击这里](https://github.com/MetaCubeX/mihomo/tree/Alpha)

adguardhome luci地址：[点击这里](https://github.com/kongfl888/luci-app-adguardhome/releases)

adguardhome核心地址：[点击这里](https://github.com/AdguardTeam/AdGuardHome)

订阅后端subconverter支持文档：[点击这里](https://github.com/tindy2013/subconverter/blob/master/README-cn.md)

规则集地址：[ACL4SSR](https://github.com/ACL4SSR/ACL4SSR/tree/master/Clash)    [blackmatrix7](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash)     [银行金融](https://github.com/StricklandF/Filter)

规则文件：[点击这里](https://drive.google.com/drive/folders/1j2uMkA_R8ppINtKkQY78gJrBJjBPtJia)

DNS泄露检测：[网站一](https://ipleak.net/)    [网站二](https://browserleaks.com/dns)


# 绕过中国大陆IP大致原理说明
 
1. 在 iptables防火墙 中添加中国大陆 ip 段直接绕过的规则（ ipset ）
2. 使用一份国内域名列表（6万条），添加至 dnsmasq 中（位置在 /tmp/dnsmasq.d 下）如果过有设置绕过黑名单会多出一个绕过域名表
比如我打开 baidu.com ，被 dnsmasq 劫持,然后它比对了“国内域名列表和绕过中国大陆黑名单”发现匹配国内域名而且不在绕过黑名单里，则使用clash中设定的大陆域名DNS 来进行解析为真实的 ip，如果 ip 与 iptables防火墙中 的 大陆ip 对应就绕过clash直接访问 ；如果在国内域名表中，但解析出来的 IP 不匹配中国 IP列表，会直接把 IP交给 clash ，以 IP 方式进行连接（可以设置域名嗅探[clash meta]来恢复成域名，从而享受到在服务器端解析，如果不设置网络嗅探，如果IP有问题，就会造成无法访问），如果是绕过黑名单里的域名，会进行DNS解析得到IP后交给clash处理，而如果我打开 google.com ，dnsmasq 发现与他的域名表不匹配也不在绕过黑名单里，不进行DNS请求,直接交给了clash 。(在大陆域名列表和绕过黑名单里的所有域名都会进行DNS解析)!!绕过大陆黑名单里填写一个国外域名也会进行DNS解析.)


# 谷歌商店下载问题，Fake-IP-Filter添加以下域名
+.services.googleapis.cn

+.xn--ngstr-lra8j.com


# subconverter后端 Docker安装：
安装命令：
docker run -d --restart=always -p 25500:25500 tindy2013/subconverter:latest

检查命令：（SSH连接openwrt执行命令）

curl http://localhost:25500/version

如果出现 subconverter vx.x.x backend 则说明容器已经成功运行。

如果容器无法启动  重启docker ！

http://localhost:25500/sub   openwrt本机docker地址

https://sub.qichiyu.com/sub  VPS 域名反代 地址

http://192.168.10.5:25500/sub 局域网其他设备地址


# BT下载必须走直连问题：
BT下载必须走直连问题。（如果最后还是有问题参考-官方建议---注意: 默认代理路由本机流量，BT、PT 下载等请尽量使用 Redir-Host 模式并注意进行流量规避，以及使用防火墙转发）
方案一，如何用的旁路有模式，单独设备或者虚拟机只做下载，然后网关指向主路由。
方案二，如果只有一个主路由，把下载机IP写入规则走直连，和让局域网设备直连一个道理。
方案三，如果同时需要BT下载直连还有科学需求-----漏网之鱼走直连。 这样规则没有匹配的所有都走了直连，或者再规则集倒数第二行添加常用端口走代理稍微弥补下。

DST-PORT,80
DST-PORT,443
DST-PORT,22

如果你使用的是你科学的这个openwrt做bt下载可以试下进程规则---只针对科学路由器本身做下载

- PROCESS-NAME,aria2c,DIRECT
- PROCESS-NAME,BitComet,DIRECT
- PROCESS-NAME,fdm,DIRECT
- PROCESS-NAME,NetTransport,DIRECT
- PROCESS-NAME,qbittorrent,DIRECT
- PROCESS-NAME,Thunder,DIRECT
- PROCESS-NAME,transmission-daemon,DIRECT
- PROCESS-NAME,transmission-qt,DIRECT
- PROCESS-NAME,uTorrent,DIRECT
- PROCESS-NAME,WebTorrent,DIRECT
- PROCESS-NAME,aria2c,DIRECT
- PROCESS-NAME,fdm,DIRECT
- PROCESS-NAME,Folx,DIRECT
- PROCESS-NAME,NetTransport,DIRECT
- PROCESS-NAME,qbittorrent,DIRECT
- PROCESS-NAME,Thunder,DIRECT
- PROCESS-NAME,Transmission,DIRECT
- PROCESS-NAME,transmission,DIRECT
- PROCESS-NAME,uTorrent,DIRECT
- PROCESS-NAME,WebTorrent,DIRECT
- PROCESS-NAME,WebTorrent Helper,DIRECT
- PROCESS-NAME,v2ray,DIRECT
- PROCESS-NAME,ss-local,DIRECT
- PROCESS-NAME,ssr-local,DIRECT
- PROCESS-NAME,ss-redir,DIRECT
- PROCESS-NAME,ssr-redir,DIRECT
- PROCESS-NAME,ss-server,DIRECT
- PROCESS-NAME,trojan-go,DIRECT
- PROCESS-NAME,xray,DIRECT
- PROCESS-NAME,hysteria,DIRECT
- PROCESS-NAME,UUBooster,DIRECT
- PROCESS-NAME,uugamebooster,DIRECT


# subconverter后端订阅转换规则说明
## [ruleset] 部分
启用自定义规则集的总开关，设置为 true 时打开，默认为 true

overwrite_original_rules

[ ] 前缀后的文字将被当作规则，而不是链接或路径，主要包含 []GEOIP 和 []MATCH(等同于 []FINAL)。

例如：

ruleset=🍎 苹果服务,https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Apple.list

@ 表示引用 https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Apple.list 规则

@ 且将此规则指向 [proxy_group] 所设置 🍎 苹果服务 策略组

ruleset=Domestic Services,clash-domain:https://ruleset.dev/clash_domestic_services_domains,86400

@ 表示引用clash-domain类型的 https://ruleset.dev/clash_domestic_services_domains 规则

@ 规则更新间隔为86400秒

@ 且将此规则指向 [proxy_group] 所设置 Domestic Services 策略组

ruleset=🎯 全球直连,rules/NobyDa/Surge/Download.list

@ 表示引用本地 rules/NobyDa/Surge/Download.list 规则

@ 且将此规则指向 [proxy_group] 所设置 🎯 全球直连 策略组

ruleset=🎯 全球直连,[]GEOIP,CN

@ 表示引用 GEOIP 中关于中国的所有 IP

@ 且将此规则指向 [proxy_group] 所设置 🎯 全球直连 策略组

ruleset=!!import:snippets/rulesets.txt

@ 表示引用本地的snippets/rulesets.txt规则

​

ruleset=💻 GitHub,clash-classic:https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/GitHub/GitHub.yaml

ruleset=🎯 国内流量,https://raw.githubusercontent.com/cutethotw/ClashRule/main/Rule/Inside.list

​

@ 会按照顺序匹配规则, .yaml文件 需要在前面添加  clash-classic:           .list 不需要

​

## [proxy_group] 部分
[ ] 前缀后的文字将被当作引用策略组

custom_proxy_group=Group_Name`url-test|fallback|load-balance`Rule_1`Rule_2`...`test_url`interval[,timeout][,tolerance]

custom_proxy_group=Group_Name`select`Rule_1`Rule_2`...

@ 格式示例

custom_proxy_group=🍎 苹果服务`url-test`(美国|US)`http://www.gstatic.com/generate_204`300,5,100

@ 表示创建一个叫 🍎 苹果服务 的 url-test 策略组,并向其中添加名字含'美国','US'的节点，每隔300秒测试一次，测速超时为5s，切换节点的延迟容差为100ms

custom_proxy_group=🇯🇵 日本延迟最低`url-test`(日|JP)`http://www.gstatic.com/generate_204`300,5

@ 表示创建一个叫 🇯🇵 日本延迟最低 的 url-test 策略组,并向其中添加名字含'日','JP'的节点，每隔300秒测试一次，测速超时为5s

custom_proxy_group=负载均衡`load-balance`.*`http://www.gstatic.com/generate_204`300,,100

@ 表示创建一个叫 负载均衡 的 load-balance 策略组,并向其中添加所有的节点，每隔300秒测试一次，切换节点的延迟容差为100ms

custom_proxy_group=🇯🇵 JP`select`沪日`日本`[]🇯🇵 日本延迟最低

@ 表示创建一个叫 🇯🇵 JP 的 select 策略组,并向其中**依次**添加名字含'沪日','日本'的节点，以及引用上述所创建的 🇯🇵 日本延迟最低 策略组

custom_proxy_group=节点选择`select`(^(?!.*(美国|日本)).*)

@ 表示创建一个叫 节点选择 的 select 策略组,并向其中**依次**添加名字不包含'美国'或'日本'的节点

​

custom_proxy_group=🇭🇰 香港节点select(?=.(香港|HK|Hong Kong|🇭🇰|HongKong))(^(?!.(家宽|游戏|剩余|流量|2.0|2倍|2x|3.0|3倍|3x|4.0|4倍|4x)).*)

@ 表示创建一个叫🇭🇰 香港节点 select 策略组,并向其中依次添加名字中含有香港|HK|Hong Kong|🇭🇰|HongKong的节点但是排除家宽|游戏|剩余|流量|2.0|2倍|2x|3.0|3倍|3x|4.0|4倍|4x

​

custom_proxy_group=🇸🇬 加坡节点select(?=.(新加坡|坡|狮城|SG|Singapore))^((?!(家宽|游戏|剩余|流量|2.0|2倍|2x|3.0|3倍|3x|4.0|4倍|4x)).)$

@ 和上一条一样的效果!! 但是这个匹配的更全面,优先使用这个,切记附带排除的前面不可以用    (新加坡|坡|狮城|SG|Singapore)    这种包含规则,必须要用正向表达式(?=.*(新加坡|坡|狮城|SG|Singapore))    才可以,不然会删除所有节点!

​

(?=.*(0.5|0.5倍|0.5x))  这种最好用表达式  单纯(0.5|0.5倍|0.5x)  也可以,效果不好的时候换表达式!

@ (新加坡) 改成  (^新加坡)  ^ 表示字符串的开头  所以只匹配开头.

比如 节点名字是    SG|新加坡    (新加坡) 会匹配,  (^新加坡)不会匹配, 新加坡|SG  就会匹配了,一个是全匹配一个是开头匹配.  此规则还没验证!

​

还可使用一些特殊筛选条件：

`!!GROUPID=%n% 待转换链接中的第 n+1 条链接中包含的节点

`!!INSERT=%n% 配置文件中 insert_url 的第 n+1 条链接所包含的节点

`!!PROVIDER=%proxy-provider-name% 指定名称的proxy-provider

GROUPID 和 INSERT 匹配支持range,如 1,!2,3-4,!5-6,7+,8-

custom_proxy_group=g1`select`!!GROUPID=0`!!INSERT=0

@ 表示创建一个叫 g1 的 select 策略组,并向其中依次添加订阅链接中第一条订阅链接中的所有节点和配置文件中 insert_url 中的**第一个**节点

custom_proxy_group=g2`select`!!GROUPID=1

@ 表示创建一个叫 g2 的 select 策略组,并向其中依次添加订阅链接中第二条订阅链接中的所有节点

custom_proxy_group=g3`select`!!GROUPID=!2

@ 表示创建一个叫 g3 的 select 策略组,并向其中依次添加订阅链接中除了第三条订阅链接之外的所有节点

custom_proxy_group=g4`select`!!GROUPID=3-5

@ 表示创建一个叫 g4 的 select 策略组,并向其中依次添加订阅链接中第四条到第六条订阅链接中的所有节点

custom_proxy_group=v2ray`select`!!GROUP=V2RayProvider

@ 表示创建一个叫 v2ray 的 select 策略组,并向其中依次添加订阅链接中组名（tag）为 V2RayProvider 的所有节点

注意：此处的订阅链接指 default_url 和 &url= 中的订阅以及单链接节点（区别于配置文件中 insert_url）

现在也可以使用2个条件组合来进行筛选，只有同时满足这2个筛选条件的节点才会被加入组内

custom_proxy_group=g1hkselect!!GROUPID=0!!(HGC|HKBN|PCCW|HKT|hk|港)

@ 属于订阅链接中的第一条订阅且名字含 HGC、HKBN、PCCW、HKT、hk、港 的节点

​

custom_proxy_group=g1hkselect!!GROUPID=0!!(HGC|HKBN|PCCW|HKT|hk|港)(?!.*(深港|家宽))

@ 属于订阅链接中的第一条订阅且名字含 HGC、HKBN、PCCW、HKT、hk、港 的节点并且排除深港\家宽

​

custom_proxy_group=g1hkselect!!GROUPID=1-2!!(HGC|HKBN|PCCW|HKT|hk|港)(?!.*(深港|家宽))

@ 属于订阅链接中的第二\三条订阅且名字含 HGC、HKBN、PCCW、HKT、hk、港 的节点并且排除深港\家宽

​

custom_proxy_group=g1hkselect!!GROUPID=!0!!(HGC|HKBN|PCCW|HKT|hk|港)(?!.*(深港|家宽))

@ 属于订阅链接中除了第一条订阅且名字含 HGC、HKBN、PCCW、HKT、hk、港 的节点并且排除深港\家宽
