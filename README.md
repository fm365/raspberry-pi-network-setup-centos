Raspberry PI Network Setup CentOS
=================================

A project to make setuping centos through network easier<br />

一个让网络安装centos更轻松的项目.<br />

使用树莓派 + Archlinux + tftp + dhcp + vsftp 来实现.<br />
你只需要做的事就是搞一个ks.cfg, 放到指定的位置, 然后带着树莓派和USB供电线去机房就OK了.<br />

下载地址
================================

* (含centos5.3安装程序, 密码请修改ks.cfg文件, 大小: 5.85G) http://kuai.xunlei.com/d/ikrmAgI6GgCfDUtSbbe
* (纯净版, 即不含/srv/ftp/iso目录下的安装镜像, 大小: 529.1M) http://kuai.xunlei.com/d/ikrmAgKbGgAPFUtS6e4

简要安装说明
================================
* 解压后是一个.img文件, 使用dd拷到你的SD卡上即可, 上电即可使用
* 如果你下载的是精简版, 你还需要使用类似gparted的软件调整下分区大小, 以便有足够的空间可以在/srv/ftp/iso目录下面放安装镜像的解压缩的文件
* 用户名和密码都是保持默认的root/root

相关配置内容
================================
* tftp, ftp的存储目录位于 /srv
* /srv/ftp/ 下面放置ks.cfg
* /srv/ftp/iso 下面放置centos的安装iso解压后的文件
* /srv/tftp 下面放置网络启动需要的引导文件
* /srv/tftp/pxelinux.cfg/default 该配置文件中配置ks文件的网络地址, 如果你修改来raspberry pi的ip的话, 就需要修改这里
* /etc/dhcpd.conf DHCP的配置文件, 默认分配 192.168.100.1 到 192.168.100.200 的 IP, 也就是说默认是只支持这么多机器同时安装
* /usr/lib/systemd/system/tftpd.service 可以修改tftp的目录, 默认目录/srv/tftp
* /etc/conf.d/network@eth0 可以修改本机的IP地址, 子网掩码, 广播地址, 网关地址, 默认值分别是: 192.168.100.250, 24, 192.168.100.255, 192.168.100.250

相关的控制命令
================================
* tftp -- systemctl status tftpd.socket
* vsftp -- systemctl status vsftpd.service
* dhcp -- systemctl status dhcpd4.service

其他
================================
* tftp, vsftp, dhcp已经设置为开机自启动了
* ks.cfg 的生成请注意CentOS版本, 不同版本的CentOS的软件包可能有所出入, 不正确的软件包选择会导致自动安装停止
* 现在还没有测试效率如何, 感觉树莓派的I/O是瓶颈, 不过现在给一台电脑装的话没啥响应速度问题
* 在考虑是否有必要开发个网页界面用来显示安装进度, 毕竟现在看不多安装的进度
