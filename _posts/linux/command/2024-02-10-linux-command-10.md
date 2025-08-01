---
layout: post
title: Linux命令系列10:定时任务
date: 2024-02-10 06:00:00 +0800
categories: [Linux命令系列]
tags: [Linux命令系列]
---


```bash

crontab
 
crontab -e 编辑当前用户的cron服务

crontab -l 列出当前用户的cron服务的详细内容

crontab -l -u elesos //列出用户elesos的所有调度任务

crontab -u 设定某个用户的cron服务

crontab -r 删除当前用户的cron服务


例如当前登陆的用户是root
运行crontab -e 会给root用户创建一个计划任务；并进入vi编辑计划任务内容。
分     小时     日      月       星期        命令

0-59  0-23    1-31   1-12         0-6       command(一般一行对应一个任务)


第1个数字表示分钟（0-59），

第2个数字表示小时（0-23），

第3个数字表示天（1-31），

第4个数字表示月份（1-12），

第5个数字表示星期（0-6），其中0表示周日。后面便是你要执行的任务。
各部分之间使用空格分开。
时间除了使用数字外还有几个特殊符号：

“*”表示所有数值 ，如第一位使用* 表示每分钟

“/”表示每， 如第一位使用 */1 表示每1分钟

“-”表示数值范围，“ ，”用来隔开离散的数值，如第2位 是1-6，8 表示1点到6点，还有8点。
注释加#号即可
启动与停止
/sbin/service crond status 查看状态

/sbin/service crond start  启动cron服务

/sbin/service crond stop  停止服务

/sbin/service crond restart  重启服务

docker里面启动

whereis crond

/usr/sbin/crond start

示例[编辑]
每10秒执行：定时任务里最好写绝对路径
*/1 * * * * /opt/nginx/html/starRTC_web/aec/doc/convert_doc.sh

*/1 * * * * (sleep 10; /opt/nginx/html/starRTC_web/aec/doc/convert_doc.sh)

*/1 * * * * (sleep 20; /opt/nginx/html/starRTC_web/aec/doc/convert_doc.sh)

*/1 * * * * (sleep 30; /opt/nginx/html/starRTC_web/aec/doc/convert_doc.sh)

*/1 * * * * (sleep 40; /opt/nginx/html/starRTC_web/aec/doc/convert_doc.sh)

*/1 * * * * (sleep 50; /opt/nginx/html/starRTC_web/aec/doc/convert_doc.sh)

每天的0点0分执行
0 0 * * * /opt/nginx/db_bak.sh


每隔1分钟向文件输出”hello elesos”

*/1 * * * * echo "hello elesos" >> /data/leijh/test.txt

可通过tail -f text.txt进行验证，或查cron的日志

tail -f /var/log/cron


每天1点向某个文件写入一段话

0 1 * * * echo "hello elesos" >> /tmp/test.txt



输出重定向[编辑]
> /dev/null 2>&1   # 最好在执行的命令最后面加上这个。不然会在/var/spool/clientmqueue目录下产生大量日志文件，而耗尽空间（开启了cron，而cron中执行的程序有输出内容，输出内容会以邮件形式发给cron的用户，而sendmail没有启动所以就产生了这些文件）


0：标准输入

1：标准输出

2：标准错误信息输出

如，将某个程序的错误信息输出到log文件中：./program 2>log 这样标准输出还是在屏幕上，但是错误信息会输出到log文件中
另外，也可以实现0，1，2之间的重定向。2>&1 ：将错误信息重定向到标准输出。
如果想要正常输出和错误信息都不显示，则要把标准输出和标准错误都重定向到/dev/null， 如：

ls 1>/dev/null 2>/dev/null

还有一种做法是将错误重定向到标准输出，然后再重定向到 /dev/null，如：
ls >/dev/null 2>&1


注意：此处的顺序不能更改，否则达不到想要的效果



*/1 * * * *  /opt/adobe/fms/webroot/api/svn.sh
一般加上> /dev/null 2>&1 
上面是每隔1分钟自动更新。下面是每隔10s
*/1 * * * * /opt/adobe/fms/webroot/api/svn.sh
*/1 * * * * (sleep 10; /opt/adobe/fms/webroot/api/svn.sh)
*/1 * * * * (sleep 20; /opt/adobe/fms/webroot/api/svn.sh)
*/1 * * * * (sleep 30; /opt/adobe/fms/webroot/api/svn.sh)
*/1 * * * * (sleep 40; /opt/adobe/fms/webroot/api/svn.sh)
*/1 * * * * (sleep 50; /opt/adobe/fms/webroot/api/svn.sh)
注意空格
重启服务 /sbin/service crond restart
crond.service: Unit crond.service not found.
service cron start

```
## 参考
http://os.51cto.com/art/201011/233945.htm
http://os.51cto.com/art/201003/187722.htm
http://aub.iteye.com/blog/1326200
https://my.oschina.net/u/2391658/blog/1525941
https://blog.csdn.net/shawnhu007/article/details/50971084
https://blog.csdn.net/soi_yu/article/details/82977220