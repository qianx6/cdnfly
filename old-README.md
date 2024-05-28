# cdnfly-kaixin
仅支持CentOS 7.x

web目录为云端验证文件，请自行搭建

1、wget https://raw.githubusercontent.com/freejbgo/cdnfly-kaixin/main/web/web.tar.gz

tar -zxvf web.tar.gz

2、wget https://raw.githubusercontent.com/freejbgo/cdnfly-kaixin/main/datetime

mkdir /web目录/common

cp datetime /web目录/common/


注意：验证服务器用bt安装，并且php版本为5.6


0.0.0.0改成(自己搭建的验证服务器IP，主控全部需要设置，节点只需要设置auth)

vi /etc/hosts

0.0.0.0 auth.cdnfly.cn (主控和节点都要设置)

0.0.0.0 monitor.cdnfly.cn（只需主控设置）

0.0.0.0 update.cdnfly.cn (注意：如果cdnfly官网挂了再设置，需要在主控和节点都设置，并且需要在安装之前就需要设置)

0.0.0.0 dl2.cdnfly.cn (注意：如果cdnfly官网挂了再设置，需要在主控和节点都设置，并且需要在安装之前就需要设置)


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



问题解决--持续更新中：


1、普通用户的账户中心中的bug修复：

cd /opt/cdnfly/master/panel/src/views/account

删除三个文件：balance、log、order

然后用下面命令重新生成软链接：

ln -s ../finance/balance/ balance

ln -s ../system/log/ log

ln -s ../finance/order/ order


2、删除/tmp/send_log/

rm -rf /tmp/send_log/


3、因为开心版的带宽和流量统计是每秒执行一次任务，所以需要替换成每隔5分钟执行一次任务。

在/opt/cdnfly/master/tasks目录中覆盖bandwidth_monitor.so文件

