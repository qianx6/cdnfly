# cdnfly-kaixin
仅支持CentOS 7.x

web目录为云端验证文件，请自行搭建

wget https://raw.githubusercontent.com/freejbgo/cdnfly-kaixin/main/web/web.tar.gz

tar -zxvf web.tar.gz



注意：验证服务器用bt安装，并且php版本为5.6


0.0.0.0改成(自己搭建的验证服务器IP，主控全部需要设置，节点只需要设置auth)

vi /etc/hosts

0.0.0.0 auth.cdnfly.cn (主控和节点都要设置)

0.0.0.0 monitor.cdnfly.cn（只需主控设置）


主控登录地址为: http://主控IP/

管理员账号和密码： wenjian/wenjian

普通用户账号和密码： ceshi/ceshi



备注：

监控默认是使用云端服务器去请求CDN节点，因此要保持云端和CDN节点之间的网络畅通。另外如果是用宝塔面板，php不要安装bt_safe扩展，否则无法使用tcp类型监控；如果要用ping类型监控，还需要允许exec函数。

支持多节点监控（和官方一样），要添加其它监控节点，可以编辑config.php配置文件，根据里面的注释说明添加。



v5.1.13主控

curl -fsSL https://github.com/freejbgo/cdnfly-kaixin/raw/main/master.sh -o master.sh && chmod +x master.sh && ./master.sh --es-dir /home/es

或者

curl -fsSL https://github.com/freejbgo/cdnfly-kaixin/raw/main/master.sh -o master.sh && chmod +x master.sh && ./master.sh --ver v5.1.13 --es-dir /home/es


v5.1.16被控

curl -fsSL https://github.com/freejbgo/cdnfly-kaixin/raw/main/agent.sh -o agent.sh  && chmod +x agent.sh && ./agent.sh --master-ver v5.1.13 --master-ip 1.2.3.4 --es-ip 1.2.3.4 --es-pwd 12345678






