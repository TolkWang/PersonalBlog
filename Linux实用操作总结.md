# Linux实用操作总结

日程工作中使用到的命令记录，不定期更新

#### linux文件系统描述

lib 动态库

etc 配置文件

usr 装应用程序

proc,sru,sys 高级

dev 设备

media 识别设备

mnt 挂载别的文件系统

opt 存放安装软件

usr/local 安装过的软件存放路径

var 日志

selinux 安全子系统，控制程序访问

##### 常用命令（更新中）

文本编辑  vi  按i,o,a,r进入插入模式, :wq保存退出

添加用户  useradd

切换用户 su -用户名

运行级别 init 0-6

创建空文件  touch

拷贝 cp -r [原目录] [目标目录]

移除 rm -r 递归删除整个文件夹 -f 强制删除不提示

mv 移动文件或者重命名

cat 查看文件内容 -n 显示行号 -more 分页显示 -less 查看大型文件（日志等）

echo "文件" 》 文件 追加内容到指定文件

head -n 5 XXX 显示指定文件的前5行

tail -f 实时追踪该文件的所有更新

ln 软连接（创建快捷方式）ln -s /root linktoroot

history 查看已经执行过的命令

find [搜素范围] [选项] 文件查找

locate 快速定位文件路径

grep 过滤查找  cat 1/txt | grep -n 15

tar -zcvf a.tar.gz 1.txt 2.txt 压缩

tar -zxvf a.tar.gz -c /home/  解压文件至指定目录

ls -ahl 查看文件所有者

chown [用户名] [文件名]  修改文件的所有者

chmod u=rwx,g=rx,o=rw 1.txt 修改权限 r:读权限 w:写权限 x:执行权限 通过数字变更 r=4,w=2,x=1  eg.chmod 777

crontab 任务调度 -e编辑 -l查看 -r 删除

ps 进程管理 -a 终端的所有信息 -u 以用户的格式显示 -x 显示后台进程的参数 eg. ps -aux | grep XXX

kill -9(进程号) 终止进程

systemctl 服务管理

chkconfig 可以给每个服务的各个运行级别设置自启动   chkconfig --level 5 [服务名] on/off