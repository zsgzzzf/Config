# =====sing-box=====

重点注意：
每项出站必须有具体节点或者直连，不然无法运行或者报错！！

homeproxy插件下载地址：https://fantastic-packages.github.io/packages/

homeproxy完美配置文件下载地址：https://github.com/qichiyuhub/rule/tree/master/config/singbox

规则模板地址：https://github.com/qichiyuhub/rule/tree/master/config/singbox

视频中演示openwrt固件下载：https://firmware-selector.immortalwrt.org/

规则集地址：https://github.com/MetaCubeX/meta-rules-dat/tree/sing/geo

自定义规则集两个文件下载地址：https://github.com/qichiyuhub/rule/tree/master/config/singbox

URL合并网站：https://www.urlencoder.org/

自定义规则集json格式检查对错：https://www.json.cn/

Json转换二进制命令：
sing-box rule-set compile --output /rules.srs /rules.json

# =====配置文件=====
示例：
https://sing-box-subscribe-doraemon.vercel.app/config/订阅链接地址&file=规则模板json地址


https://sing-box-subscribe-doraemon.vercel.app/config/hysteria2://7ea6ae68-2e01-457f-8aa7-7e680cead7a4@vps.morverthink.eu.org:32779?peer=vps.morverthink.eu.org%26insecure=0%26sni=vps.morverthink.eu.org%26alpn=h3|https://my.vipuser.info/sub-rl?port=20925%26passwd=sDpECu&file=https://raw.githubusercontent.com/zsgzzzf/Config/refs/heads/main/singbox/config_mixed.json

singbox在线模板：

https://raw.githubusercontent.com/zsgzzzf/Config/refs/heads/main/singbox/config_tun.json
https://raw.githubusercontent.com/zsgzzzf/Config/refs/heads/main/singbox/config_mixed.json
https://raw.githubusercontent.com/zsgzzzf/Config/refs/heads/main/singbox/config_fakeip.json

docker版后端：

docker run -d --name sing-box-subscribe -p 5000:5000 hestudy/sing-box-subscribe

http://localhost:5000/config/

网站版后端：

https://github.com/Toperlock/sing-box-subscribe/blob/main/instructions/README.md

singbox Wiki：
https://sing-box.sagernet.org/zh/configuration

json格式检查：
https://www.json.cn/

裸核运行：

mkdir /etc/sing-box

cd /etc/sing-box

./sing-box run -c config.json
